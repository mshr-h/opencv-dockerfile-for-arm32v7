# Build OpenCV Python wheel package for ARM32v7

This repository provides [OpenCV on Wheels](https://github.com/opencv/opencv-python) build dockerfile for PYNQ-Z1 and other ARM32v7 devices.

## Build instruction

1. Install DockerCE on your machine by following the instruction
    - [https://docs.docker.com/install/](https://docs.docker.com/install/)

2. Clone this repository

```bash
git clone https://github.com/mshr-h/opencv-dockerfile-for-arm32v7
cd opencv-dockerfile-for-arm32v7
```

3. Modify Python version to your device environment in `Dockerfile.arm32v7`

```dockerfile
ENV PYTHON_VERSION 3.9.5 # -> change it for your environment
```

4. Run docker build

```bash
docker build -t opencv-arm32v7 -f Dockerfile.arm32v7 .
```

5. Note the fullpath of the `.whl` files

```bash
docker create -it --name cv_temp opencv-arm32v7 bash
docker start cv_temp
docker exec -it cv_temp ls /code/opencv-python
```

The output looks like the below.
Note the name of the wheel files. (`opencv_contrib_python-4.5.2+aa26990-cp39-cp39-linux_armv7l.whl` and `numpy-1.20.2-cp39-cp39-linux_armv7l.whl`)

```
CONTRIBUTING.md        find_version.py                                                 pyproject.toml
LICENSE-3RD-PARTY.txt  multibuild                                                      setup.py
LICENSE.txt            numpy-1.20.2-cp39-cp39-linux_armv7l.whl                         tests
MANIFEST.in            opencv                                                          travis_config.sh
README.md              opencv_contrib                                                  travis_multibuild_customize.sh
appveyor.yml           opencv_contrib_python-4.5.2+aa26990-cp39-cp39-linux_armv7l.whl  travis_osx_brew_cache.sh
cv2                    patch_auditwheel_whitelist.py
docker                 patches
```

6. Copy the wheel files from the docker image

```bash
docker cp cv_temp:/code/opencv-python/opencv_contrib_python-4.5.2+aa26990-cp39-cp39-linux_armv7l.whl .
docker cp cv_temp:/code/opencv-python/numpy-1.20.2-cp39-cp39-linux_armv7l.whl .
docker rm -fv cv_temp
```

7. Copy the wheel files (`opencv_contrib_python-4.5.2+aa26990-cp39-cp39-linux_armv7l.whl` and `numpy-1.20.2-cp39-cp39-linux_armv7l.whl`) to PYNQ-Z1 or other ARM32v7 device

8. Install the wheel files

```bash
sudo apt update
sudo apt install -y python3 python3-pip
pip3 install numpy-1.20.2-cp39-cp39-linux_armv7l.whl
pip3 install opencv_contrib_python-4.5.2+aa26990-cp39-cp39-linux_armv7l.whl
```
