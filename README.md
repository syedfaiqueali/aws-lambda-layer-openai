# aws-lambda-layer-openai

This repository provides steps to create a **custom AWS Lambda layer** for the ```OpenAI``` library, including the latest version .zip file for easy deployment.


## Step 1: Create a virtual environment
Create a Python virtual environment and activate it

```bash
# Create the aws-lambda-layer directory and navigate into it
mkdir aws-lambda-layer
cd aws-lambda-layer

# Create a virtual environment named 'myenv' with Python 3.10
python3.10 -m venv myenv

# Activate the virtual environment
source ./myenv/bin/activate
```

## Step 2: List your dependencies in requirements.txt
The ```OpenAi``` Python library depends on quite a few other libraries. Make sure to list all of them in the ```requirements.txt``` file along with their desired versions. This ensures that all their built distributions are installed with respect to your targeted platform. Place your **requirements.txt** file inside the above directory **aws-lambda-layer**

## Step 3: Install built distributions (wheels)
To download a wheel that’s compatible with Lambda, you use the pip --platform option.

If your Lambda function uses the **x86_64** instruction set architecture, run the following ```pip install``` command to install a compatible wheel in your package directory. Replace ```--python 3.x``` with the version of the Python runtime you are using.

```bash
pip install \
--platform manylinux2014_x86_64 \
--target=package \
--implementation cp \
--python-version 3.10 \
--only-binary=:all: --upgrade \
-r requirements.txt
```

If your Lambda function uses the **arm64** instruction set architecture, run the following ```pip install``` command to install a compatible wheel in your package directory. Replace ```--python 3.x``` with the version of the Python runtime you are using.

```bash
pip install \
--platform manylinux2014_aarch64 \
--target=package \
--implementation cp \
--python-version 3.10 \
--only-binary=:all: --upgrade \
-r requirements.txt
```

## Step 4: Deactivate the environment
```bash
deactivate
```

## Step 5: Create a .zip file for the downloaded packages
```bash
# Create the 'python' folder
mkdir python

# Copy all contents from 'package' to 'python'
cp -r package/* python/

# Zip the 'python' folder
zip -r <zip-file-name>.zip python
```

## Step 6: Upload the .zip file as custom layer over aws console
1. Navigate to Lambda -> Layers
2. Press **Create Layer**
3. Write your layer name, description
4. Upload **.zip file**
5. Choose compatible architectures *(x86_64, arm64)*
6. Choose compatible runtimes *(Python3.10, Python3.11, Python3.12)*
7. Press Create