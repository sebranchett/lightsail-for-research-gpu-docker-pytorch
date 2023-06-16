# Combination of GPU, Docker and PyTorch working on Amazon Lightsail for Research

Almost nothing in this repository is original. If you find something that is, then it is supplied under an [MIT license](./LICENSE).

## Sources

### Installing Docker Engine on Ubuntu:
- https://docs.docker.com/engine/install/ubuntu/
- https://docs.docker.com/engine/install/linux-postinstall/

Documentation used under an [Apache-2.0 license](https://github.com/docker/docs/blob/main/LICENSE).

### Installing NVIDIA container toolkit:
- https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html

© Copyright 2018-2023, NVIDIA Corporation. Last updated on 2023-06-10.

### NVIDIA Docker container:
- https://catalog.ngc.nvidia.com/orgs/nvidia/containers/pytorch.
See [End User License Agreement](https://developer.nvidia.com/ngc/nvidia-deep-learning-container-license)

### PyTorch example:
- https://pytorch.org/tutorials/beginner/basics/quickstart_tutorial.html - © Copyright 2022, PyTorch, and used under an [Attribution 4.0 International (CC BY 4.0) license](https://creativecommons.org/licenses/by/4.0/)

## Amazon Lightsail for Research
> If you have access to [DelftBlue](https://doc.dhpc.tudelft.nl/delftblue/), and want to do something similar there, please see [this repository](https://github.com/sebranchett/delftblue-gpu-apptainer-pytorch/tree/main).

If you are new to Amazon Lightsail for Research, you may find [this 10 minute video](https://www.youtube.com/watch?v=3QF_N21e_oc) useful, and you can find the user guide [here](https://docs.aws.amazon.com/lightsail-for-research/latest/ug/what-is-lfr.html).

### Teething troubles
Amazon Lightsail for Research is a relatively new service.
As of June 2023, you can use the standard, CPU, plans without any special actions. If you want to use GPU plans, you need to submit a quota request. On my first attempt at starting a GPU machine, I got the message:
```
Your instance cannot be created at this time.
InvalidInputException: You reached the quota of total vCPUs that you can create in your account.
Your virtual computers can have a combined total of 0 vCPUs.
Delete virtual computers that you don't need, or request a quota increase.
https://docs.aws.amazon.com/general/latest/gr/lightsail.html#limits_lightsail
```
This was confusing, as my Lightsail quota was 20 vCPUs. The advice I got from the Amazon support team is:
```
As of now there is no specific quota name for this quota increase request. It’s just that Lightsail for Research has stricter restrictions, and GPU access is restricted by default. However raising a Lightsail quota increase request along with the error encountered will further assist us in taking the right course of action.
```
I filed a quota request by logging into my AWS account, searching for 'Service Quotas', clicking on 'AWS Services', searching for 'Lightsail', then 'Instances' and then clicking on 'Request quota increase'. It took a few days before I got access to the GPU plans.

## First time setup
Using the console, create a Ubuntu virtual computer with a GPU XL plan, in the region of your choice. This can take around 20 minutes. Once started, launch Ubuntu and open the terminal. Replicate this repository and run the installation script:
```
git clone https://github.com/sebranchett/lightsail-for-research-gpu-docker-pytorch.git
cd lightsail-for-research-gpu-docker-pytorch
./installation.sh
```
The installation script will install Docker Engine and the NVIDIA container toolkit. It will then test a CUDA container and run a PyTorch example. After completion, the file `quickstart.log` should end with:
```
Done!
Saved PyTorch Model State to model.pth
Predicted: "Ankle boot", Actual: "Ankle boot"
```
as described in the [PyTorch documentation](https://pytorch.org/tutorials/beginner/basics/quickstart_tutorial.html).

> Don't forget to stop the virtual computer once you've finished.

## Next steps
Consider creating a 'Cost Control' rule for your virtual computer. The default settings will automatically stop the computer if the CPU has been idle for 5 minutes. This could save you a lot of money.

Consider making a 'Snapshot' of your virtual computer, just after you're happy with the installation. You can then use this Snapshot to start a new virtual computer in the future, perhaps of a different size.