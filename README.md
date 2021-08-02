# Python Online and Offline ECG QRS Detector based on the Pan-Tomkins algorithm 

## Authors
* Michał Sznajder (Jagiellonian University) - technical contact (msznajder@gmail.com)
* Marta Łukowska (Jagiellonian University)
 
## Intro

The modules published in this repository are Python implementations of online and offline QRS complex detectors in ECG signal, based on the Pan-Tomkins algorithm (Pan J., Tompkins W. J., _A real-time QRS detection algorithm,_ IEEE Transactions on Biomedical Engineering, Vol. BME-32, No. 3, March 1985, pp. 230-236).

The QRS complex corresponds to the depolarization of the right and left ventricles of the human heart. It is the most visually obvious part of the ECG signal. QRS complex detection is essential for time-domain ECG signal analyses, namely heart rate variability. It makes it possible to compute inter-beat interval (RR interval) values that correspond to the time between two consecutive R peaks. Thus, a QRS complex detector is an ECG-based heart contraction detector.

You can find out more about cardiac cycle and QRS complex [here](https://en.wikipedia.org/wiki/Cardiac_cycle) and [here](https://en.wikipedia.org/wiki/QRS_complex).

This repository contains two versions of the Pan-Tomkins QRS detection algorithm implementation:
* __QRSDetectorOnline__ - Online version detects QRS complexes in a real-time acquired ECG signal. Therefore, it requires an ECG device to be plugged in and receiving a signal in real-time.
* __QRSDetectorOffline__ - Offline version detects QRS complexes in a pre-recorded ECG signal dataset (e.g. stored in _.csv_ format).

__This implementation of a QRS Complex Detector is by no means a certified medical tool and should not be used in health monitoring. It was created and used for experimental purposes in psychophysiology and psychology.__

이 저장소에 게시된 모듈은 Pan-Tomkins 알고리즘(Pan J., Tompkins W.J., _A 실시간 QRS 검출 알고리즘, _IEEE Transactions on Biomedical Engineering, Vol. BME-32, 3월 3일)에 기반한 심전도 신호의 온라인 및 오프라인 QRS 복합 검출기의 파이썬 구현 모듈이다.

QRS 컴플렉스는 인간 심장의 우심실 및 좌심실의 탈분극에 해당합니다. 심전도 신호의 가장 시각적으로 명확한 부분입니다. QRS 복합 검출은 시간 영역 심전도 신호 분석, 즉 심박수 변동성에 필수적입니다. 두 연속 R 피크 사이의 시간에 해당하는 RR 구간(비트 간 간격) 값을 계산할 수 있습니다. 따라서 QRS 복합 검출기는 심전도 기반 심장 수축 검출기입니다.

심장 주기 및 QRS 복합체에 대한 자세한 내용은 [여기](https://en.wikipedia.org/wiki/Cardiac_cycle) 및 [여기](https://en.wikipedia.org/wiki/Q)를 참조하십시오.RS_복합).

이 리포지토리에는 두 가지 버전의 Pan-Tomkins QRS 탐지 알고리즘 구현이 포함되어 있습니다.
* __QRSDetectorOnline__ - 온라인 버전은 실시간 획득 심전도 신호에서 QRS 복합체를 감지합니다. 따라서 심전도 장치를 꽂고 실시간으로 신호를 수신해야 합니다.
* __QRSDetectorOffline__ - 오프라인 버전은 사전 기록된 심전도 신호 데이터 집합(예: _.csv_ 형식으로 저장)에서 QRS 복합체를 탐지합니다.

__QRS 복합 검출기의 이러한 구현은 결코 인증된 의료 도구가 아니며 건강 모니터링에 사용해서는 안 된다. 그것은 정신생리학 및 심리학의 실험 목적으로 만들어지고 사용되었습니다.__

## Algorithm

The published QRS Detector module is an implementation of the QRS detection algorithm known as the Pan-Tomkins QRS detection algorithm, first described in a paper by Jiapu Pan and Willis J. Tomkins titled _"A Real-Time QRS Detection Algorithm"_ (1985). The full version of the paper is accessible [here](http://www.robots.ox.ac.uk/~gari/teaching/cdt/A3/readings/ECG/Pan+Tompkins.pdf).

The direct input to the algorithm is a raw ECG signal. The detector processes the signal in two stages: filtering and thresholding.

First, in the filtering stage each raw ECG measurement is filtered using a cascade of low-pass and high-pass filters that together form a band-pass filter. This filtering mechanism ensures that only parts of the signal related to heart activity can pass through. The filters eliminate most of the measurement noise that could cause false positive detection. The band-pass filtered signal is then differentiated to identify signal segments with high signal change values. These changes are then squared and integrated to make them more distinct. In the last step of the processing stage, the integrated signal is screened by a peak detection algorithm to identify potential QRS complexes within the integrated signal.

In the next stage, the identified QRS complex candidates are classified by means of dynamically set thresholds, either as QRS complexes or as noise peaks. The thresholds are real-time adjusted: a threshold in a given moment is based on the signal value of the previously detected QRS and noise peaks. The dynamic thresholding accounts for changes in the noise level. The dynamic thresholding and complex filtering ensure sufficient detection sensitivity with relatively few false positive QRS complex detections.

Importantly, not all of the features presented in the original Pan and Tomkins paper were implemented in this module. Specifically, we decided not to implement supplementary mechanisms proposed in the paper that are not core elements of QRS detection. Therefore, we did not implement the following features: fiducial mark on filtered data, use of another set of thresholds based on the filtered ECG, irregular heart rate detection, and missing QRS complex detection search-back mechanism. Despite the lack of these supplementary features, implementation of the core features proposed by Pan and Tompkins allowed us to achieve a sufficient level of QRS detection. 

## 알고리즘

공개된 QRS 검출기 모듈은 Jiapu Pan과 Willis J의 논문에서 처음 설명한 Pan-Tomkins QRS 검출 알고리즘으로 알려진 QRS 검출 알고리즘을 구현한 것이다. 톰킨스는 '실시간 QRS 검출 알고리즘'(1985)이라는 제목을 붙였다. 본 문서의 전체 버전은 [여기]에서 확인할 수 있습니다(http://www.robots.ox.ac.uk/~gari/teaching/cdt/A3/readings/ECG/Pan+Tompkins.pdf).

알고리즘에 대한 직접 입력은 원시 심전도 신호입니다. 디텍터는 필터링 및 임계값 지정의 두 단계로 신호를 처리합니다.

첫째, 필터링 단계에서 각 원시 심전도 측정은 대역 통과 필터를 구성하는 저역 통과 및 고역 통과 필터의 계단식 필터를 사용하여 필터링됩니다. 이 필터링 메커니즘은 심장 활동과 관련된 신호의 일부만 통과할 수 있도록 보장합니다. 필터는 잘못된 양의 탐지를 유발할 수 있는 대부분의 측정 노이즈를 제거합니다. 그런 다음 대역 통과 필터링된 신호를 구분하여 신호 변경 값이 높은 신호 세그먼트를 식별합니다. 그런 다음 이러한 변경 사항을 제곱하고 통합하여 보다 뚜렷하게 만듭니다. 처리 단계의 마지막 단계에서 통합 신호는 통합 신호 내의 잠재적 QRS 복합체를 식별하기 위해 피크 감지 알고리즘에 의해 선별됩니다.

다음 단계에서 식별된 QRS 복합체 후보들은 동적으로 설정된 임계값을 사용하여 QRS 복합체 또는 소음 피크로 분류된다. 임계값은 실시간으로 조정되며, 주어진 모멘트의 임계값은 이전에 탐지된 QRS 및 노이즈 피크의 신호 값을 기반으로 합니다. 동적 임계값은 노이즈 레벨의 변화를 설명합니다. 동적 임계값 지정 및 복합 필터링은 잘못된 양의 QRS 복합 탐지가 상대적으로 적으면서도 충분한 탐지 감도를 보장합니다.

중요한 것은 원본 Pan 및 Tomkins 논문에 제시된 모든 기능이 이 모듈에서 구현된 것은 아니라는 점입니다. 구체적으로, 우리는 QRS 검출의 핵심 요소가 아닌 논문에 제시된 보완 메커니즘을 시행하지 않기로 결정했습니다. 따라서 필터링된 데이터에 대한 기준 표시, 필터링된 심전도에 기반한 또 다른 임계값 세트 사용, 불규칙한 심박수 감지, 누락된 QRS 복합 검출 검색-백 메커니즘 등의 기능을 구현하지 않았다. 이러한 보완 기능이 없음에도 불구하고 Pan과 Tompkins가 제안한 핵심 기능을 구현하면 충분한 수준의 QRS 감지를 달성할 수 있었습니다.

## Dependencies

Modules published here consist of the following dependencies:
* jupyter
* matplotlib
* numpy
* pyserial
* scipy

All the dependencies are in the _requirements.py_ file. 

The modules are implemented for use with Python 3.x. However, they are relatively easy to convert to work with Python 2.x: 
- import division module with 
```
from __future__ import division
```
- import print function with
```
from __future__ import print_function
```
- remove the _decode()_ function call when reading data from an ECG device in the online version of the QRS Detector, i.e. use 
```
raw_measurement.rstrip().split(',') 
```
instead of 
```
raw_measurement.decode().rstrip().split(',')
```

## Repository directory structure 
```
├── LICENSE
│
├── README.md          				 <- The top-level README for developers using this project.
│
├── arduino_ecg_sketch 				 <- E-health ECG device Arduino sketch source code and library.
│
├── ecg_data           				 <- Pre-recorded ECG datasets in .csv format.
│
├── logs               				 <- Data logged by Online and Offline QRS Detector in .csv format.
│
├── plots          	   			 <- Plots generated by Offline QRS Detector.
│
├── qrs_detector_offline_example.ipynb  	 <- Jupyter notebook with Offline QRS Detector usage.
│
├── QRSDetectorOffline.py   			 <- Offline QRS Detector module.
│
├── QRSDetectorOnline.py    			 <- Online QRS Detector module.
│
└── requirements.txt  	 			 <- The requirements file containing module dependencies.
```

## Installation and usage

The QRS Detector module was implemented in two separate versions: Online and Offline. Each has a different application and method of use.

### Online QRS Detector

The Online version is designed to work with a directly connected ECG device. As an input it uses an ECG signal received in real-time, detects QRS complexes, and outputs them to be used by other scripts in order to trigger external events. For example, the Online QRS Detector can be used to trigger visual, auditory, or tactile stimuli. It has already been successfully implemented in PsychoPy (Peirce, J. W. (2009). Generating stimuli for neuroscience using PsychoPy (Peirce, J. (2009). _Generating stimuli for neuroscience using PsychoPy._ Frontiers in Neuroinformatics, 2 (January), 1–8. [http://doi.org/10.3389/neuro.11.010.2008](http://doi.org/10.3389/neuro.11.010.2008)) and tested to study cardioceptive (interoceptive) abilities, namely, in Schandry’s heartbeat tracking task (Schandry, R. (1981). _Heart beat perception and emotional experience._ Psychophysiology, 18(4), 483–488. 
[http://doi.org/10.1111/j.1469-8986.1981.tb02486.x](http://doi.org/10.1111/j.1469-8986.1981.tb02486.x)) and heartbeat detection training based on that proposed by Schandry and Weitkunat (Schandry, R., & Weitkunat, R. (1990). _Enhancement of heartbeat-related brain potentials through cardiac awareness training._ The International Journal of Neuroscience, 53(2-4), 243–53. [http://dx.doi.org/10.3109/00207459008986611](http://dx.doi.org/10.3109/00207459008986611)).

The Online version of the QRS Detector module has been implemented to work with the Arduino-based e-Health Sensor Platform V2.0 ECG device. You will find more about this device [here](https://www.cooking-hacks.com/documentation/tutorials/ehealth-biometric-sensor-platform-arduino-raspberry-pi-medical#step4_2).  

An Arduino e-Health ECG device sketch is also provided in this repository. The sampling rate of the ECG signal acquisition is set to 250 (Hz) samples per second. Measurements are sent in a real-time in a string format, _"timestamp,measurement"_, and have to be parsed by the QRS Detector module.

To use the Online QRS Detector module, the ECG device with the loaded Arduino sketch has to be connected to a USB port. Then QRS Complex Detector object is initialized with the port name where the ECG device is connected and the measurement baud rate is set. No further calibration or configuration is needed. The Online QRS Detector starts detection immediately after initialization.

Below is example code of how to run the Online QRS Detector:

```
from QRSDetectorOnline import QRSDetectorOnline

qrs_detector = QRSDetectorOnline(port="/dev/cu.usbmodem14311", baud_rate="115200")
```

If you want to use the Online QRS Detector in the background with other processes running on the layer above it (e.g. display a visual stimulus or play a tone triggered by the detected QRS complexes), we suggest using a Python multiprocessing mechanism. Multiprocessing offers both local and remote concurrency, effectively side-stepping the Global Interpreter Lock by using sub-processes instead of threads. Here is example code of the many ways to achieve this:

```
from QRSDetectorOnline import QRSDetectorOnline

qrs_detector_process = subprocess.Popen(["python", "QRSDetectorOnline.py", "/dev/cu.usbmodem14311"], shell=False)
```

Even though the Online QRS Detector was implemented to be used with an Arduino-based e-Health Sensor Platform ECG device with 250 Hz sampling rate and a specified data format, it can be easily modified to be used with any other ECG device, sampling rate or data format. For more information, check the "Customization" section. 


### 온라인 QRS 디텍터

Online 버전은 직접 연결된 심전도 장치와 함께 작동하도록 설계되었습니다. 입력으로 실시간 수신된 심전도 신호를 사용하고, QRS 복합체를 감지하여 다른 스크립트가 외부 이벤트를 트리거하기 위해 사용하도록 출력합니다. 예를 들어 온라인 QRS 디텍터를 사용하여 시각, 청각 또는 촉각 자극을 트리거할 수 있습니다. 그것은 이미 PsychoPy에서 성공적으로 구현되었다. PsychoPy를 사용하여 신경 과학에 대한 자극을 생성합니다(Peirce, J.(2009). _PsychoPy를 이용한 신경과학 자극 생성_ 신경정보학 분야 프론티어, 1월 2일(1월), 1~8일. [http://doi.org/10.3389/neuro.11.010.2008](http://doi.org/10.3389/neuro.11.010.2008))) 및 샨드리의 심장박동 추적 과제(Chandry, R. (positive) 심박동수능(interceptive) 능력을 연구하기 위해 테스트되었다. _심장 박동 인식과 감정 경험._ 정신생리학, 18(4), 483–488
[http://doi.org/10.1111/j.1469-8986.1981.tb02486.x](http://doi.org/10.1111/j.1469-8986.1981.tb02486.x)) 및 샨드리와 바이츠나트가 제안한 것에 기초한 심장박동 감지 훈련(Schandry, R. & Weitkunat, R.(1990). _심장인식교육을 통한 심장박동 관련 뇌 잠재력 향상_ 국제 신경과학 학술지, 53(2-4), 243-53. [http://dx.doi.org/10.3109/00207459008986611](http://dx.doi.org/10.3109/00207459008986611))).

온라인 버전의 QRS 디텍터 모듈은 Arduino 기반 e-Health 센서 플랫폼 V2.0 심전도 장치와 함께 작동하도록 구현되었습니다. 이 장치에 대한 자세한 내용은 [여기](https://www.cooking-hacks.com/documentation/tutorials/ehealth-biometric-sensor-platform-arduino-raspberry-pi-medical#step4_2)를 참조하십시오.  

이 리포지토리에는 Arduino e-Health ECG 장치 스케치도 제공됩니다. 심전도 신호 수집의 샘플링 속도는 초당 250(Hz) 샘플로 설정됩니다. 측정은 _"timestamp, measurement"_ 문자열 형식으로 실시간으로 전송되며 QRS 디텍터 모듈에서 구문 분석해야 합니다.

온라인 QRS 디텍터 모듈을 사용하려면 Arduino 스케치가 로드된 심전도 장치를 USB 포트에 연결해야 합니다. 그런 다음 QRS Complex Detector 개체가 ECG 장치가 연결되어 있고 측정 보드 속도가 설정된 포트 이름으로 초기화됩니다. 추가 보정 또는 구성이 필요하지 않습니다. 온라인 QRS 디텍터는 초기화 후 즉시 탐지를 시작합니다.

다음은 온라인 QRS 디텍터를 실행하는 방법에 대한 예제 코드입니다.

```
from QRSDetectorOnline 가져오기QRSDetectorOnline

qrs_detectors=QRSDetectorOnline(port="/dev/cu.usbmodem14311", baud_rate="115200")
```

온라인 QRS 디텍터를 백그라운드에서 실행 중인 다른 프로세스(예: 시각적 자극 표시 또는 탐지된 QRS 복합체에 의해 트리거된 신호음 재생)와 함께 사용하려면 Python 다중 처리 메커니즘을 사용하는 것이 좋습니다. 멀티 프로세싱은 로컬 및 원격 동시성을 모두 제공하므로 스레드 대신 하위 프로세스를 사용하여 Global Interpreter Lock을 효과적으로 우회할 수 있습니다. 이를 위한 다양한 방법의 예제 코드는 다음과 같습니다.

```
from QRSDetectorOnline 가져오기QRSDetectorOnline

qrs_process_process = 하위 프로세스.Popen("python", "QRSDetectorOnline.py", "/dev/cu.usbmodem³11", shell=Matrix)
```

Online QRS Detector는 250Hz 샘플링 속도와 지정된 데이터 형식의 아두이노 기반 e-Health 센서 플랫폼 심전도 장치와 함께 사용하도록 구현되었지만 다른 심전도 장치, 샘플링 속도 또는 데이터 형식과 함께 사용할 수 있도록 쉽게 수정할 수 있습니다. 자세한 내용은 "사용자 정의" 섹션을 참조하십시오.

### Offline QRS Detector

The Offline version of the detector works with ECG measurement datasets stored in _.csv_ format. These can be loaded into the detector to perform QRS detection.

The Offline QRS Detector loads the data, analyses it, and detects QRS complexes in the same way as the online version, but it uses an entire existing dataset instead of real-time measurements directly from an ECG device.

This module is intended for offline QRS detection in ECG measurements when real-time QRS-based event triggering is not crucial, but when more complex data analysis, visualisation and detection information are needed. It can also be used simply as a debugging tool to verify whether the online version works as intended, or to check the behaviour of QRS Detector intermediate data processing stages. 

The offline version of the QRS Detector module was implemented to work with any kind of ECG measurements data that can be loaded from a file. Unlike the online version, it was not designed to work with some specific device. The Offline QRS Detector expects _"timestamp,measurement"_ data format stored in _.csv_ format and is tuned for measurement data acquired with 250 Hz sampling rate. Both the format of the data and the sampling rate can be easily modified. For more information, check the "Customization" section. 

The Offline QRS Detector requires initialization with a path to the ECG measurements file. The QRS Detector will load the dataset, analyse measurements, and detect QRS complexes. It outputs a detection log file with marked detected QRS complexes. In the file, the detected QRS complexes are marked with a '1' flag in the 'qrs_detected' log data column. Additionally, the Offline QRS Detector stores detection results internally as an ecg_data_detected attribute of an Offline QRS Detector object. Optionally, it produces plots with all intermediate signal-processing steps and saves it to a *.csv* file.

Below is example code showing how to run the offline version of the QRS Detector module:

```
from QRSDetectorOnline import QRSDetectorOnline

qrs_detector = QRSDetectorOffline(ecg_data_path="ecg_data/ecg_data_1.csv", verbose=True, log_data=True, plot_data=True, show_plot=False)
```

Check _qrs_detector_offline_example.ipynb_ Jupyter notebook for an example usage of the Offline QRS Detector with generated plots and logs.

## Customization

### QRSDetectorOnline 
The Online QRS Detector module can be easily modified to work with other ECG devices, different sampling rates, and different data formats:

- QRSDetectorOnline works on a real-time received signal from an ECG device. Data is sent to the module in a _"timestamp,measurement"_ format string. If a different ECG data format is expected, the *process_measurement()* function requires some changes in order to enable correct data parsing. The Online QRS Detector works even if only measurement values without corresponding timestamps are available.

- QRSDetectorOnline is tuned for 250 Hz sampling rate by default; however, this can be customized by changing seven configuration attributes (marked in the code) according to the desired signal_frequency.  For example, to change the signal sampling rate from 250 to 125 samples per second, simply divide all the parameters by 2: set *signal_frequency* to 125, *number_of_samples_stored * to 100 samples, *integration_window* to 8 samples, *findpeaks_spacing* to 25 samples, *detection_window* to 20 samples and * refractory_period* to 60 samples.

### QRSDetectorOffline
The Offline QRS Detector is hardware independent because it uses an ECG measurements dataset instead of real-time acquired ECG signal. Therefore, the only parameters that need to be adjusted are sampling rate and data format:

- QRSDetectorOffline is by default tuned for a sampling rate of 250 samples per second. It can be customized by changing 4 configuration attributes (marked in the code) according to the desired *signal_frequency*. For example, to change signal sampling rate from 250 to 125 samples per second, divide all parameters by 2: set *signal_frequency* value to 125, *integration_window* to 8 samples, *findpeaks_spacing* to 25 samples and *refractory_period* to 60 samples.  

- QRSDetectorOffline uses a loaded ECG measurements dataset. Data is expected to be in csv format with each line in *"timestamp,measurement"* format. If a different ECG data format is expected, changes needs to be made in the *load_ecg_data()* function, which loads the dataset, or in the *detect_peaks()* function, which processes measurement values in:
```
ecg_measurements = ecg_data[:, 1] 
```
The algorithm will work fine even if only measurement values without timestamps are available.

## Citation information
If you use these modules in a research project, please consider citing it:

[![DOI](https://zenodo.org/badge/55516257.svg)](https://zenodo.org/badge/latestdoi/55516257)

If you use these modules in any other project, please refer to MIT open-source license.

## Acknowledgements
The following modules and repository were created as a part of the project “Relationship between interoceptive awareness and metacognitive abilities in near-threshold visual perception” supported by the National Science Centre Poland, PRELUDIUM 7, grant no. 2014/13/N/HS6/02963. Special thanks for Michael Timberlake for proofreading. 

