# SolidiFI
SolidiFI is an automated and systematic framework for evaluating smart contracts' static analysis tools

For more details about SolidiFI, please reference the paper  [How Effective are Smart Contract Analysis Tools? Evaluating Smart Contract Static Analysis Tools Using Bug Injection](http://blogs.ubc.ca/karthik/2020/05/22/how-effective-are-smart-contract-static-analysis-tools-evaluating-smart-contract-static-analysis-tools-using-bug-injection/)

The dataset and the results of the paper are available [here](https://github.com/DependableSystemsLab/SolidiFI-benchmark)

If you use SolidiFI, please cite this paper

 ```
@inproceedings{ghaleb2020effective,
  title={How Effective Are Smart Contract Analysis Tools? Evaluating Smart Contract Static Analysis Tools Using Bug Injection},
  author={Ghaleb, Asem and Pattabiraman, Karthik},
  booktitle={Proceedings of the 29th ACM SIGSOFT International Symposium on Software Testing and Analysis},
  year={2020}
}
  ```
## Quick Start
 A docker container with required dependencies is available for easy use of SolidiFI. 
 
  Download the docker image (WARNING: This can take some time.):
  ```
   docker pull asemg/solidifi
  ```
  
   Run the container:
  ```
   docker run -i -t asemg/solidifi
  ```
 ## Usage From Source
 
 SolidiFI requires Python 3.6+. 
   
 ### 1. Clone source coode
   
   ```
   git clone --recursive https://github.com/DependableSystemsLab/SolidiFI.git
   ```
 
  ### 2. Install dependencies

 To use SolidiFI from your machine, please run the following command to install depedencies required for SolidiFI to work properly.
 
   ```
    pip install --upgrade pip
    pip install -r requirements.txt
    pip install -e .
    curl -L https://github.com/ethereum/solidity/releases/download/v0.5.12/solc-static-linux > /usr/bin/solc-0.5.12 && \
        chmod +x /usr/bin/solc-0.5.12 && \
        ln -s /usr/bin/solc-0.5.12 /usr/local/bin/solc
   ```
   
## Using SolidiFI to Inject Bugs into a Smart Contract
 SolidiFI can be used to inject bugs into a contract as follows 
   
  ```
   python3 solidify.py -i <Path-to-contract-source-file> <Bug-type>
  ```
  For the Bug-type parameter, it can be one of the following
	[Re-entrancy, Timestamp-Dependency, Unchecked-Send, Unhandled-Exceptions, TOD, Overflow-Underflow, tx.origin]
  
   For example to inject Timestamp Dependency bugs into /solidifi/contracts/1.sol smart contract 
  
  ```
  python3 solidifi.py -i /solidifi/contracts/1.sol Timestamp-Dependency
  ```
    
  The generated buggy contract along with the injection log will be stored under the "buggy/Timestamp-Dependency" folder.
  
  ## Tools Evaluation Using SolidiFI 
   
   In case you want to to evaluate the analysis tools mentioned in the paper from scratch. You can run  evaluator.py.
   
   This script will inject bugs of the different 7 bug types in the contracts dataset (stored in the folder "contracts"). Then it will scan the generated buggy contracts using the six analysis tools being evaluated, and finally, it will inspect the analysis reports of the tools for false negatives, false positives, and misidentified bugs.
   
   ```
   python3 evaluator.py Oyente,Securify,Mythril,Smartcheck,Manticore,Slither
   ``` 
  
   ## Extending the Set of The Bug Types
   
   SolidiFI is already configured to inject the bug types mentioned above. However, SolidiFI can be extended to inject other bugs.
   
   To configure SolidiFI to inject other bugs, update the configuration file named "bug_types.conf" by adding the following pattern for  each bug.
   
        [<id>]
	bug_type_id = <id>
	bug_type = <type>
	bug_type_dir = <folder-containing-bug-snippets>

   After updating the bugs configuration file, create a folder under "bugs" directory and name it by the name specified in "bug_type_dir" specifed in th configuration file. Inside this folder, create two subfolders, namely, "ts" and "tf" and put all non-block bug snippets under the "ts" folder" and the block bug snippets, written as functions or other blocks, under the folder "tf"
