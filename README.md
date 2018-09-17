# Self-driving-car---simulated

Here I will try and put the code from my model and the trained network with weights and paramters to be used in 
the autonomous mode in the simulator.

# Working progress

- Managed to get the car to run in autonomous mode with the network I've trained
- I've tried to use Hanssons model with the same hyperparameters without getting much success with the autonomous mode in the simulator, using his data. Could this be because his data is not good enough? Not good enough recovery data included? I trained my network on data I created myself which includes recovery data. This dataset is collected on the jungle track - should probably try and do the same thing on the lake track to get good results there since it's an easier track.

# To do

- Learn git better

# Thougts and things tested

- It doesn't seem to matter if I put the drop out rate (keep_prob) to 0 or 1!!!

# Commands and other useful stuff
To change all occurences of /IMG to /jungle_dataset_1_lap_plus_recover/IMG in a text (csv) file. 

sed -i 's='/IMG'='/jungle_dataset_1_lap_plus_recover/IMG'=g' driving_log_jungle_linux_merged.csv



