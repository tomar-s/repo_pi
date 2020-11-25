# repo_pi
Note:The folder where my project resides is "/users/pgrad/tomars/"i.e. project root folder. 
Followed the steps for access GPU access from: http://support.scss.tcd.ie/index.php/SCSS_GPU_Resources

******Created virtual environment on Pi- rasp-035.berry.scss.tcd.ie and installed required packages********
mkdir shivani_sc
python3 -m venv shivani_sc/
source shivani_sc/bin/activate
pip install scikit-build
pip install openvc-python
pip install tflite
NOTE: need to install tflite_runtime in Pi
pip install https://github.com/google-coral/pycoral/releases/download/release-frogfish/tflite_runtime-2.5.0-cp38-cp38-linux_armv7l.whl


****** Generated training and validation dataset using below commands ******
NOTE: changed required parameters accordingly
generate.py --width 128 --height 64 --length 1 --symbols symbols.txt --count 2000 --output-dir training_data


****** Training the Model on generated dataset using slurm on scss Lab GPUs ******
ssh tomars@slurm-master.scss.tcd.ie
./register_job.sh python shivani_train/train.py --width 128 --height 64 --length 6 --batch-size 32 --train-dataset shivani_train/training_data --validate-dataset shivani_train/validation_data --output-model-name shivani_train/test_model_us --epochs=15 --symbols shivani_train/symbols.txt


****** Running Tensorboard ******
source /opt/conda/etc/profile.d/conda.sh
conda activate tf-gpu
tensorboard --logdir logs

********Adding code to git repo***********
First go into the folders which has all the files you need to put in a repo:
run the following commands from git cmd(local)/linux(labmachine):
git init
git add .
git commit -m "commit message"

****** Running Classify to decode captchas ******
NOTE: activate virtual env
python classifyPi.py  --model-name test_model_mc --captcha-dir tomars_demo_captcha --output livedemo_output.txt --symbols symbols.txt


****** shell script to run on Pi(autoplay_20.sh) ******
activates the virtual environment 
clones the required code from git repository
executes classifyPi.py 
