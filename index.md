---
layout: default
---

<!-- ## Why should we care about Smart Contract Vulnerability? -->
<!--[title](url "Hover title")-->
<span style="display:block;text-align:center">
![logo](./assets/images/logo2.png "Project Logo" ){:height="400px"}
</span>
## What are Smart Contracts?

Smart Contracts are computer programs that execute on a blockchain. The nature of blockchains allows one to run Smart Contracts in a trustless and decentralized environment. While different projects implement the concept of Smart Contracts, we concentrate on EVM-based blockchains and use Ethereum as our primary example, as it is the most popular, adopted, and advanced implementation. 

At first glance, those Smart Contracts seem rather abstract -- code running in a VM on a blockchain. However, they provide the underlying technology in a vast and fast-growing ecosystem of [NFTs](https://www.theverge.com/22310188/nft-explainer-what-is-blockchain-crypto-art-faq "NFTs explained"), [decentralized applications](https://ethereum.org/en/dapps/ "Ethereum dApps"), and -- of course -- [CryptoKitties](https://www.cryptokitties.co/ "CryptoKitties Website"). All of those systems have an invested interest of million and even billion dollars. Ethereum itself has a market cap of over 250 billion USD. Furthermore, all of those systems use the fundamental promise of trustless execution, where no trusted 3rd parties are needed to establish trust between two strangers on the Internet.

Generally, Smart Contracts for EVM-compatible blockchains are written in a scripting language and compiled into so-called bytecode. Then, the bytecode is included in the blockchain via a transaction and executed by the miners (or validators in blockchains with stacking-based consensus algorithms). Once the bytecode is on the blockchain, everybody can see the Smart Contract's bytecode and even issue transactions to execute functionalities of them.

### Why should we care about Smart Contract vulnerabilities?

The inherently public nature of deployed Smart Contracts and the possible financial incentives make Smart Contracts perfect targets for malicious actors. Historically, attackers were successful with their attacks and could steal millions, for example, exploiting the [DAO contract](https://www.gemini.com/cryptopedia/the-dao-hack-makerdao#section-the-dao-hack "DAO Hack"). Not only that, but vulnerabilities in Smart Contracts undermine the benefits of trustless execution. For example, the other party could plant vulnerabilities in Smart Contracts to exploit them later and invalidate the intent of the Smart Contract to steal money.

### Why use ML and Transfer Learning?

To detect vulnerabilities in Smart Contracts, researchers and developers proposed many detection tools ([Mythril](https://github.com/ConsenSys/mythril "Mythril GitHub Repository"), [Oyente](https://github.com/enzymefinance/oyente "Oyente GitHub Repository"), [MadMax](https://github.com/nevillegrech/MadMax "MadMax GitHub Repository"), [Vandal](https://github.com/usyd-blockchain/vandal "Vandal GitHub Repository"), etc.). Most of those tools apply static and dynamic code analysis in addition to fuzzing. While the security community widely adopted those tools, most of them have a substantial execution time of several seconds to minutes. Furthermore, specific tools only allow testing for a subset of potential security issues, which leads to a need to run and test Smart Contracts with multiple different tools. As a result, Smart Contract developers and users would need to deploy, understand and manage several security tools to test for most vulnerability classes.

In recent years classification and labeling of pictures, text, and also malware using Machine Learning arose. As such, the detection of vulnerabilities in Smart Contracts using Machine Learning is the next logical step towards a more secure ecosystem. Furthermore, Machine Learning allows us to speed up the detection processes and combine different tools into one classification process. ML models enable developers and users to test Smart Contract bytecode near-instant without deploying a complex vulnerability detection.

In classical Machine Learning, the training process is bound to a pre-defined set of labels and classes. So, once trained, a Machine Learning model can only detect an unchanging set of vulnerabilities. To alleviate this restriction, we apply the concept of Transfer Learning. Once a model is trained, we can reuse it to modify its labels (i.e., outputs) or even inputs without retraining the entire model. Transfer Learning, therefore, makes the ML model extendable and cuts back at overall training time.

## Our Work towards Security of Smart Contracts

In our work, we demonstrate the effectiveness of Transfer Learning in the domain of Smart Contract vulnerabilities. Specifically, we propose to use Transfer Learning to enable the extensibility of our Machine Learning model in regards to vulnerability classes. Moreover, we show the clear benefit of Transfer Learning by successfully classifying even underrepresented vulnerability classes.

### ESCORT [1,2]

We developed a practical tool-chain to apply Transfer Learning based on vulnerability classes of Smart Contracts in this work. Our high-level execution flow can be seen in this graphic:

<span style="display:block;text-align:center">
![logo](./assets/images/overview_escort.png "Overview Workflow" ){:width="600px"}
</span>

We first label the smart contracts applying different smart contract vulnerability detection tools bundled in our **Demeter** -- each posses scan capabilities for different vulnerability types. Then, we train, test, and validate our extensible multi-label model with **ESCORT**. Suppose developers find new vulnerabilities or extend the capabilities of current tools at a later point in time. In that case, we can only train on new and underrepresented data to achieve a well-performing model. During deployment, developers can quickly test their smart contracts with **ESCORT**. 

#### Demeter

<span style="display:block;text-align:center">
![logo](./assets/images/toolchainUpd.png "Overview ContractScraper" ){:width="600px"}
</span>

The high-level flow of **Demeter** is straightforward. First, **Demeter** extracts the addresses of smart contracts and then their bytecode from the blockchain network, including test networks. For this task, we included several different tools, such as [Geth](https://geth.ethereum.org/ "Geth"), [Erigon](https://github.com/ledgerwatch/erigon "Erigon GitHub") and [Google BigQuery](https://cloud.google.com/bigquery/?hl=de&utm_source=google&utm_medium=cpc&utm_campaign=emea-de-all-de-dr-bkws-all-all-trial-e-gcp-1011340&utm_content=text-ad-none-any-DEV_c-CRE_253480395030-ADGP_Hybrid%20%7C%20BKWS%20-%20EXA%20%7C%20Txt%20~%20Data%20Analytics%20~%20BigQuery-KWID_43700053287067648-kwd-63326440124-userloc_2276&utm_term=KW_google%20bigquery-NET_g-PLAC_&gclid=CjwKCAjw3POhBhBQEiwAqTCuBtrslaXlyDJG-wwkL88_rjzWqM5QHhBZ2cywgmipg4b68SCFo-zolBoCZQ0QAvD_BwE&gclsrc=aw.ds "BigQuery by Google"). With the plain bytecode, we scan the bytecode for vulnerabilities using [Mythril](https://github.com/ConsenSys/mythril "Mythril GitHub Repository"), [Vandal](https://github.com/usyd-blockchain/vandal "Vandal GitHub Repository") and [Oyente](https://github.com/enzymefinance/oyente "Oyente GitHub Repository"). We also already preprocess the bytecode for Machine Learning, applying different rules to prepare the bytecode for inclusion into **ESCORT**.
In total, the bytecode of 3.6 million Smart Contracts is classified by the three detection tools, allowing us to label Smart Contracts with eight unique vulnerability classes. 

#### ESCORT 

<span style="display:block;text-align:center">
![logo](./assets/images/overview_ml.png "Overview Escort" ){:width="600px"}
</span>

As seen above, our high-level model architecture consists of two parts. The first part is the *Feature Extractor*. Here, the model learns the fundamental structure of smart contract bytecode. The second part is the *Vulnerability Branches*. Here, we can extend the model with new vulnerability branches exclusive to a single vulnerability class. We train our model first with six different classes and then use Transfer Learning to extend our model to two additional classes. Here, we achieved a substantially lower inference time of 0.02s, improved training time, and a mean F1-score of 97%. ESCORT can then be used to classify bytecode in an on-the-fly manner.

#### Dataset

The dataset used in our evaluation will be shared for research purposes. Interested parties can fill out the release form for our ESCORT dataset. After review, the dataset will be shared via Google Drive, so please state with which Google account the files should be shared. You can find the release form [here](./assets/files/release_agreement_escort.pdf "Release Form").

## Publications
 * [1] **_ESCORT: Ethereum Smart COntRacTs Vulnerability Detection using Deep Neural Network and Transfer Learning_** by Oliver Lutz (University of Würzburg), Huili Chen ([University of California, San-Diego](https://sites.google.com/eng.ucsd.edu/huilichen/home)), Hossein Fereidooni ([TU Darmstadt](https://www.informatik.tu-darmstadt.de/systemsecurity/people_sys/people_details_sys_48576.en.jsp)), Christoph Sendner ([University of Würzburg](https://se.informatik.uni-wuerzburg.de/secure-software-systems-group/staff0/christoph-sendner/)), Alexandra Dmitrienko ([University of Würzburg](https://se.informatik.uni-wuerzburg.de/secure-software-systems-group/staff0/alexandra-dmitrienko/)), Ahmad Reza Sadeghi ([TU Darmstadt](https://www.informatik.tu-darmstadt.de/systemsecurity/people_sys/people_details_sys_45184.en.jsp)), and Farinaz Koushanfar ([University of California, San Diego](https://farinaz.eng.ucsd.edu/home)). Paper available as **[pre-print](https://arxiv.org/pdf/2103.12607.pdf)**.

 * [2] **_Smarter Contracts: Detecting Vulnerabilities in Smart Contracts with Deep Transfer Learning_** by Christoph Sendner ([University of Wuerzburg](https://se.informatik.uni-wuerzburg.de/secure-software-systems-group/staff0/christoph-sendner/)), Huili Chen ([University of California, San-Diego](https://sites.google.com/eng.ucsd.edu/huilichen/home)), Hossein Fereidooni ([TU Darmstadt](https://www.informatik.tu-darmstadt.de/systemsecurity/people_sys/people_details_sys_48576.en.jsp)), Lukas Petzi ([University of Wuerzburg](https://se.informatik.uni-wuerzburg.de/secure-software-systems-group/staff0/lukas-petzi/)), Jan König (University of Wuerzburg), Jasper Stang (University of Wuerzburg), Alexandra Dmitrienko ([University of Wuerzburg](https://se.informatik.uni-wuerzburg.de/secure-software-systems-group/staff0/alexandra-dmitrienko/)), Ahmad Reza Sadeghi ([TU Darmstadt](https://www.informatik.tu-darmstadt.de/systemsecurity/people_sys/people_details_sys_45184.en.jsp)), and Farinaz Koushanfar ([University of California, San Diego](https://farinaz.eng.ucsd.edu/home)). Paper available as **[conference paper](https://dx.doi.org/10.14722/ndss.2023.23263)**.
