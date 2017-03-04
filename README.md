# Docker Developer - Beginner Linux Containers

Following along the Docker training course https://training.docker.com

Writing down all what I will execute in the Command Line, plus some key concepts and tips, will help me to retain, in the long run, more from what I have learned during this training.

Try to do the same and you will notice that after some weeks or months your memory still remember more easily what you have learned.

> #### TIP:
>   Avoid using `sudo docker` by aliasing it:
>   
>   ```bash
>   echo 'alias docker="sudo docker"' >> ~/.bashrc # or for ZSH ~/.zshrc
>   ```


# Index

* [Setup](training/setup.md)
* [Training 1.0 - Running Your First Container](training/1.0/running-your-first-container.md)
    + [Docker Pull](training/1.0/running-your-first-container.md#docker-pull)
    + [Docker Images](training/1.0/running-your-first-container.md#docker-images)
    + [Docker Run](training/1.0/running-your-first-container.md#docker-run)
    + [Docker Ps](training/1.0/running-your-first-container.md#docker-ps)
    + [Docker Terminology](training/1.0/running-your-first-container.md#docker-terminology)
* [Training 2.0 - Web Apps With Docker](training/2.0/run-a-static-website-in-a-container.md)
    + [Run a Static Website in a Container](training/2.0/run-a-static-website-in-a-container.md)
        + [Docker Run](training/2.0/run-a-static-website-in-a-container.md#docker-run)
        + [Docker Port](training/2.0/run-a-static-website-in-a-container.md#docker-port)
        + [Docker Run - Advanced Usage](training/2.0/run-a-static-website-in-a-container.md#docker-run-advanced-usage)
        + [Docker Stop](training/2.0/run-a-static-website-in-a-container.md#docker-stop)
        + [Docker Rm](training/2.0/run-a-static-website-in-a-container.md#docker-rm)
    + [Explaining Docker Images](training/2.0/explaining-docker-images.md)
        + [Images Tags](training/2.0/explaining-docker-images.md#images-tags)
        + [Images types](training/2.0/explaining-docker-images.md#images-types)
    + [Create Your First Docker Image](training/2.0/create-your-first-docker-image.md)
        + [Writing a DockerFile](training/2.0/create-your-first-docker-image.md#writing-a-dockerfile)
        + [Docker Build](training/2.0/create-your-first-docker-image.md#docker-build)
        + [Docker Push](training/2.0/create-your-first-docker-image.md#docker-push)
        + [Dockerfile Commands Explained](training/2.0/create-your-first-docker-image.md#dockerfile-commands-explained)
    + [Create a Python Flask App in a Docker Image](training/2.0/create-a-python-flask-app-in-a-docker-image.md)
        + [Write the App](training/2.0/create-a-python-flask-app-in-a-docker-image.md#write-the-app)
        + [Build a Docker Image With The App](training/2.0/create-a-python-flask-app-in-a-docker-image.md#build-a-docker-image-with-the-app)
        + [Run the App Container](training/2.0/create-a-python-flask-app-in-a-docker-image.md#run-the-app-container)
        + [Publish Your App Image](training/2.0/create-a-python-flask-app-in-a-docker-image.md#publish-your-app-image)
