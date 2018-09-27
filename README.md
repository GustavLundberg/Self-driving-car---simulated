# Self-driving-car---simulated

Here I will try and put the code from my model and the trained network with weights and paramters to be used in 
the autonomous mode in the simulator.

# Working progress

- Managed to get the car to run in autonomous mode with the network I've trained
- I've tried to use Hanssons model with the same hyperparameters without getting much success with the autonomous mode in the simulator, using his data. Could this be because his data is not good enough? Not good enough recovery data included? I trained my network on data I created myself which includes recovery data. This dataset is collected on the jungle track - should probably try and do the same thing on the lake track to get good results there since it's an easier track.
-  Trained the Hansson network (same hyperparameters, only 2 epochs) on jungle track (own data, 1 lap + recover). Result:
Lake track, manages to drive until red and white stuff on the ashfell enters. Jungle track, kör in till vänster i första svängen (kör banan baklänges) - klara sedan inte den första vänstersvängen.

- Vid träning av lake_merged så hittar den inte filen /lunarc/nobackup/projects/lu-also/Lundberg/datasets/lake_track_multiple_laps_plus_recover
/IMG_merged/center_2018_09_17_14_11_11_665.jpg, kan det ha skett något fel vid mergen av de 4 dataseten (lake, reversed, recover, reversed recover)?

- model_lake_2 är tränad på (ganska säker) 2ep med hanssons nätverk på lake track och den klarar av att köra flertalet varv på lake track med hög hastighet (tror den klarar throttle=1, dvs 30 km/h). Den fixar dock INTE jungle track.

- När samma nätverk som ovan används och tränas i 25 ep så väljs modellerna ut som blir efter 16ep och 20ep (dessa har lägst validation error) ut för att testas i simulatorn. Resultatet blir en katastrof, bilen kör men konstant vinkel (-typ 0.16). Överträning trots att validation error är litet??

- To do #4 nedan. Precis testat modellen efter ep0, ep1, ep2 på samma nätverk (kallad model_lake_5ep_ep0_low_learrate.h5 (ep1, ep2 också). Vid hastigheter runt 20 är performance på lake jämbördiga, 0ep oscillerar faktiskt mindre än ep1, ep2. Vid throttle=1 är det dock bara ep2 som klarar att köra runt flera varv på lake, dock med STORA oscillationer. Alla tre nätverken funkar kasst på jungle track, dvs även sämre än den som heter model_lake_2 som klarar första svängarna och som är tränad med högre learning rate än i detta fallet.

- Lagt till recover from recover data (rfr) till lake. Detta ger bäst resultat hitills vid testning på lake track, enbart små oscillationer! Tränad i 20 ep, resultatet på lake blir i princip bara bättre och bättre så man skulle kunna testa att träna längre och förhoppningsvis få ännu bättre resultat! På jungle track är resultatet mycket dåligt och detta nätverk skulle behöva kombineras med någon slags transfer learning för att funka på jungle track.

# To do

1) Learn git better
2) Starta med att träna på lake track data för att se om modellen lär sig nånting. Om den lär sig nånting kan vi konstatera att inlärningen beror på datasetet. 
3) Testa om den klarar hela lake track på 15 km/h. Bumpa upp till 20 km/h eller högre och se om det funkar.
4) Testa att köra de tränade modellerna i simuleringen efter 1ep, 2ep osv för att se hur performance i simulatorn hänger ihop med validation error. Sker det någon överträningen? Hur ser överträning i simulatorn ut-kör den rakt fram?
5) Testa därefter om man kan träna på lake track och testa på jungle track, dvs transfer learning (en autoencoder kan tydligen behövas enligt Sopasakis).
6) Lägg till hastighet i modellen först när det andra funkar!

Idé:
- Göra någon slags variant av transfer learning för att träna på (mycket) data från lake och sen transfera den modellen och fintuna på (lite) data från jungle track?


# Thougts and things tested

- It doesn't seem to matter if I put the drop out rate (keep_prob) to 0 or 1!!!

# Trained networks on lake data

- 20ep, 40 batchsize, drop_prob=0.5, samples_per_epoch=50000, lear_rate=0.0001, test_size = 0.2 ger:
loss: 0.0396 - acc: 0.2684 - val_loss: 0.0283 - val_acc: 0.6636
Testning på lake: dåliga resultat. Efter några ep så verkar det vara överträning då bilen oscillerar mycket fram och tillbaka. Klarar inte varvet på max hastighet. 
Testning på jungle: kasst, klarar ingenting.
Slutsats: Verkar ske överträning, mer regularization?

(Samma som ovan fast med mer dropout-lager)
- 20ep, 40 batchsize, drop_prob=0.5, samples_per_epoch=50000, lear_rate=0.0001, test_size = 0.2 (lagt till 4 lager dropout!) ger:
Testning på lake: Ger ganska dåliga resultat, verkar inte klara av banan på throttle=1. Starka oscillationer eller att den svänger för hårt och kör av på andra sidan vägen.
Testning på jungle (låg hastighet - throttle delas med ca 2.5): ep17 klarar att köra upp till kraftiga vänstersvängen med rödvita pinnar på högersidan.

# Commands and other useful stuff
To change all occurences of /IMG to /jungle_dataset_1_lap_plus_recover/IMG in a text (csv) file. 

sed -i 's='/IMG'='/jungle_dataset_1_lap_plus_recover/IMG'=g' driving_log_jungle_linux_merged.csv

# Links

Transfer learning
https://medium.com/@14prakash/transfer-learning-using-keras-d804b2e04ef8
https://www.learnopencv.com/keras-tutorial-fine-tuning-using-pre-trained-models/

Notice there's a difference between Sequential API and Functional API, where it seems like the Sequential isn't made for working with multiple outputs.
https://machinelearningmastery.com/keras-functional-api-deep-learning/



