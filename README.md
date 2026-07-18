## __ddG Prediction__

### Installation & setup
Choose the environment configuration that matches your hardware capabilities.

__Create the environment__

__For NVIDIA GPU (CUDA) acceleration__
  ```bash
      conda env create -f environment_GPU.yaml
  ```
__For CPU-only execution__
  ```bash
      conda env create -f environment_CPU.yaml
  ```

__Activate the environment__
  ```bash
      conda activate predictDDG
  ```

__Structure preprocessing guide__
It is highly recommended to prepare and clean your PDB files before training or running predictions.

* Residue numbering: Ensure mutation positions match the specific Residue ID (auth_seq_id) inside the PDB file, not the zero-indexed positional array index.

* Isolate chains: Keep only the relevant target protein chain, remove heteroatoms (HETATM), and clean up formatting artifacts using pdb-tools:

```bash
    pip install pdb-tools
    pdb_selchain -A 1SHF.pdb | pdb_delhetatm | pdb_tidy > Input_structures/1shf_A.pdb
```

### Predicting new data
To predict mutational effects using a pre-trained model, download the contents of the Prediction folder.

__Required directory structure__
Ensure your working folder contain all these files/folder before executing the script:
* predict_ddG.py
* Input_data_for_prediction.csv 
  * The input CSV file must contain a comma-separated header block matching this schema:
  * ```bash
      PDB_file,Chain,WT,Position,Mutation
      1bz6.pdb,A,L,29,N
      1bz6.pdb,A,V,68,N
      1bz6.pdb,A,F,123,T
  ```
* Folder 'Input_structures' : PDB files must be in this folder name 

__Execution__
```bash
# Specifying custom model paths and datasets explicitly
python 03_predict_ddG.py --model path/to/model.pt --input test_predict.csv
```
__Expected output__
The script generates a new results file prefixed with Predictions_:
Example output name: Predictions_test_predict.csv

