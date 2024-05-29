# WebGuard: Detecting Evasive Web Scanners via a Spatio-Temporal Forensics Engine

## Abstract
Web traffic has evolved to include both human users and automated agents, ranging from benign web crawlers to adversarial scanners such as those capable of credential stuffing, command injection, and account hijacking at the web scale. The estimated financial costs of these adversarial activities are estimated to exceed tens of billions of dollars in 2023. In this work, we introduce WebGuard, a low-overhead in-application forensics engine, to enable robust identification and monitoring of automated web scanners, and help mitigate the associated security risks. WebGuard focuses on the following design criteria: (i) integration into web applications without any changes to the underlying software components or infrastructure, (ii) minimal communication overhead, (iii) capability for real-time detection, e.g., within hundreds of milliseconds, and (iv) attribution capability to identify new behavioral patterns and detect emerging agent categories. To this end, we have equipped WebGuard with spatio-temporal behavioral monitoring mechanisms for browser events. We also design supervised and unsupervised learning architectures for real-time detection and offline attribution of human and automated agents, respectively. Information theoretic analysis and empirical evaluations are provided to show that the proposed data analysis, as opposed to similar analysis which relies solely on mouse movement dynamics, significantly improves time-to-detection and attribution accuracy. Various numerical evaluations using real-world data collected via WebGuard are provided achieving high accuracy in hundreds of milliseconds, with a communication overhead below 10 KB per second.


## Repository Structure
The repository includes different modules introduced in the corresponding paper. You can find details of each module in the following.

### Collector
The collector module includes both `server` and `client` codes of the data collection process. Details of the collector module can be found in a dedicated [README](collector/README.md) file included in the directory.

### Data
We have provided sample traces from each of the experimented agent types. Additionally, `data` directory includes the required codes for preprocessing of the raw json files to be used in future analysis.

### HMM
The code to regenrate the `HMM` model and reproduce the related experiments are provided in `hmm` directory. The experiments include clustering and classification of the traces.

### LSTM
The utilized `LSTM` model architecture as well as the required codes for training and test phases are available in `lstm` directory. 

