# pxlinux-ci
CI Environment for building PX:Linux Images.

## About PX:Linux

[PX: Linux](https://www.pxtech.io) is a Linux Distribution for Industrial IoT system based on [Arch Linux ARM](https://archlinuxarm.org/).

PX:Linux is part of PX:Tech, an End-to-End platform for building Industrial IoT systems including:
- [PX:Board](https://www.pxtech.io/pxboard/index.html) A production ready Single Board Computer enabled with LTE 4G.
- [PX:Linux](https://www.pxtech.io/pxboard/index.html) A Linux distribution for Industrial IoT systems.
- [PX:Cloud](https://www.pxtech.io) A cloud-based platform that allows for effective IoT fleet management.

## Building the Docker image

You can download a prebuilt docker image from [Docker Hub](https://cloud.docker.com/u/pxtech/repository/docker/pxtech/pxlinux-ci). To pull the image simply run:
```
docker pull pxtech/pxlinux-ci
```

To build it yourself, clone this [Github repository](https://github.com/pxtechio/pxlinux-ci) and run the build command:

```

docker build .
```

## Preparing the run

Before running the image, make sure you have a valid config/targets.yaml file. You can specify more than one target.
### Mandatory tags
#### Targets
- Name: Name of your target.
- TargetImage: Name of the file that will be created.
- SkipUpdate: Boolean value. Make true if you want to skip package updates.
- Assets/BaseImage: Path to base image that will be updated.

#### Commands
Currently, only build command is supported:
BuildCommand:
  cmd: helper_scripts/build_img.sh
  params:
    - BaseImage
    - TargetImage
    - U-Boot
    - DeviceTrees
    - Packages
    - Scripts
    
### Optional
- Assets/U-Boot: Path to custom U-Boot file. This will be injected from sector 1.
- Assets/DeviceTrees: Path to .dtb files that will be copied to /boot directory. Symlinks are NOT updated by this stage.
- Assets/Packages: Path to packages that will be installed as part of the new image. A pacman_packages.txt can be provided to install from configured pacman repos.
- Assets/Scripts: Path to custom scripts to modify base image. This stage can perform updates to services, fstab, bootscript, etc.

### Minumal config
A minimal targets.yaml file can look like this:


## Running
Container must run privileged because it needs to create and use loopback devices to mount and modify the .img files.
Directories containing 'assets' and 'config' must be mounted as volumes.

```
docker run 				\
	--user=root 			\
	-it 				\
	--rm 				\
	--privileged 			\
	-v ~/pxlinux/assets:/assets 	\
	-v ~/pxlinux/config:/config 	\
	pxtech/pxlinux:latest
```

## Contribute
If you're interested in adding features or contributing to PX:Linux or PX:Tech, contact the maintainers of this repository.

