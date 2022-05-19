clear;clc;

name_dossier = '20220308-ibidi-smURFP-0-5uM-BVMe2-duration';
name_file = 'osmolarity_in_time_exp_1';

%% PUMP parameters (DO NOT TOUCH)
duration_replace_medium = 3; % second %must be as little as possible
siringe_size = 29; % 29 mm diameter syringe 50mL
%siringe_size = 20.1; % 20.1 mm diameter syringe 20mL
siringe_rate = 6; % 6 mL/min
seconds_before_acq = 120; % second (IMAGE 2 min on NIKON before start the code)

%% EXPERIMENTS parameters
pulse_duration = 90; % second
%pulse_duration = 5; % second
%pulse_duration = 20; % second

pulse_period = 2; %min
number_of_pulse = 5;

% Conditions
%%% Find the refractory time
%%%%%% pulse_duration_default = 20 sec;
%%%%%% pulse_period = 30 sec, 1 min, 2 min, 5 min, 10 min, 15 min

%%%%%% pulse_duration = 5 sec, 10 sec, (20 sec), 1 min, 2 min, 5 min
%%%%%% pulse_period = refractory time

%% SAVE DATA parameters

max_time = (seconds_before_acq/60) + number_of_pulse*pulse_period + (pulse_duration/60) + 2; %min %+2 is the additional minutes
max_time = [0 0 0 0 max_time 0]; %max_time = [year month day hour minute seconds] %%%% TOUT METTRE EN MINUTE %%%%

%% (Do not touch) Save condition
pathChloe = 'C:\Users\RouxLab\Desktop\Chloe';
[status, msg, msgID] = mkdir([pathChloe filesep name_dossier]);
transfo_sec = [0 0 0 60*60 60 1];

iso = 315;
hypo = 160;
ramp_step = (iso-hypo)/duration_replace_medium;
vec_ramp_iso_hypo = [iso iso-ramp_step iso-2*ramp_step hypo];
vec_pulse_duration = hypo*ones(1,pulse_duration);
vec_ramp_hypo_iso = flip(vec_ramp_iso_hypo);
vec_pulse_period = 315*ones(1,(pulse_period*60)-pulse_duration-2*duration_replace_medium);
vec_one_pulse = [vec_ramp_iso_hypo vec_pulse_duration vec_ramp_hypo_iso vec_pulse_period];

number_pulse = ceil(sum(max_time.*transfo_sec)/size(vec_one_pulse,2));
vec_x_pulse = repmat(vec_one_pulse,1,number_pulse);
osmolarity_in_time = [315*ones(1,seconds_before_acq) vec_x_pulse];

save([pathChloe filesep name_dossier filesep name_file '.mat'],'osmolarity_in_time','-v7.3');

%% Set-up pumps

% PUMP1
pumpLEI = SyringePump('COM15'); %COM7
%pumpLEI.SetDiameter(siringe_size,pumpLEI_id);
pumpLEI.SetDiameter(siringe_size);
pumpLEI.SetRate(siringe_rate,'MM'); % 10 ml/min

% PUMP2
pumpH20 = SyringePump('COM13'); %COM6
pumpH20.SetDiameter(siringe_size);
%pumpH20.SetDiameter(siringe_size,pumpH20_id);
pumpH20.SetRate(siringe_rate,'MM'); % 10 ml/min

%% EXECUTIVE TASK 2
% Apply consecutive stress
% Use the clock to stop acquisition

% Translate period in seconds
pulse_period_sec = pulse_period*60-duration_replace_medium-pulse_duration-duration_replace_medium; %second

% Time of started experiment
time_start = clock;
count_pulse = 0;

%while((sum(clock.*transfo_sec)-sum(time_start.*transfo_sec))<sum(max_time.*transfo_sec))==1
while (count_pulse<number_of_pulse)==1
    
    count_pulse = count_pulse +1;

    fprintf('Start osmotic shock\n')
    pumpH20.Start();
    pause(duration_replace_medium);
    fprintf('Stop osmotic shock\n')
    pumpH20.Stop();
    
    pause(pulse_duration)
    
    fprintf('Start LEI\n')
    pumpLEI.Start();
    pause(duration_replace_medium);
    fprintf('Stop LEI\n')
    pumpLEI.Stop();
    
    pause(pulse_period_sec)
end

%% Info
% Link pump diameter
% https://www.air-tite-shop.com/Images/document/Inner%20Diameter%20HSW%20Syringes%20Slip%20and%20Lock.pdf
