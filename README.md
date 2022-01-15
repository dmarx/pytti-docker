
mkdir -p /opt/projdata

cd ~/proj/pytti-docker

./download_models.sh

sudo docker build -t pytti:test .
#sudo docker run --rm -it -p 8181:8181 --gpus all -v /opt/projdata:/opt/colab/images_out pytti:test
#sudo docker ps


docker-compose -f /opt/clearml/docker-compose.yml up -d

#clearml-agent list
clearml-agent daemon --queue art --create-queue --docker pytti:test  --detached

# CLEARML_AGENT_SKIP_PIP_VENV_INSTALL=true ...
clearml-task --project PYTTI --name remote_cli_test --script ~/proj/pytti-docker/pytti_cli_w_clearml.py --queue art --packages pip 
# --skip-task-init