#Base image (Alpine is minimalistic)
FROM python:3.6.4-alpine3.7

# install requirements
RUN apk update && apk upgrade    
COPY src/requirements.txt /tmp/requirements.txt
RUN pip install -r /tmp/requirements.txt

# copy current directory to /basic
COPY . /basic

# set basic/src as the working directory
WORKDIR /basic/src

# Run the python file
CMD ["python","-u","Basic.py"]


















