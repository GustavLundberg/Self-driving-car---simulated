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

# To do

- Learn git better
- Starta med att träna på lake track data för att se om modellen lär sig nånting. Om den lär sig nånting kan vi konstatera att inlärningen beror på datasetet. 
- Testa om den klarar hela lake track på 15 km/h. Bumpa upp till 20 km/h eller högre och se om det funkar.
- Testa att köra de tränade modellerna i simuleringen efter 1ep, 2ep osv för att se hur performance i simulatorn hänger ihop med validation error. Sker det någon överträningen? Hur ser överträning i simulatorn ut-kör den rakt fram?
- Testa därefter om man kan träna på lake track och testa på jungle track, dvs transfer learning (en autoencoder kan tydligen behövas enligt Sopasakis).
- Lägg till hastighet i modellen först när det andra funkar!


# Thougts and things tested

- It doesn't seem to matter if I put the drop out rate (keep_prob) to 0 or 1!!!

# Commands and other useful stuff
To change all occurences of /IMG to /jungle_dataset_1_lap_plus_recover/IMG in a text (csv) file. 

sed -i 's='/IMG'='/jungle_dataset_1_lap_plus_recover/IMG'=g' driving_log_jungle_linux_merged.csv



