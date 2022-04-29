# How to check parallel i/o speeds and performance on ubuntu 

## Install fio 

- fio stands for flexible I/O tester 
- used for benchmarking and stress/hardware verification
- can be installed directly using apt
- parallel I/O inbuilt

To install
```bash
sudo apt install fio
```

## Performing random write test
```bash
sudo fio --name=randwrite --ioengine=libaio --iodepth=1 --rw=randwrite --bs=4k --direct=0 --size=512M --numjobs=2 --runtime=240 --group_reporting
```

## Performing random read test
```bash
sudo fio --name=randread --ioengine=libaio --iodepth=16 --rw=randread --bs=4k --direct=0 --size=512M --numjobs=4 --runtime=240 --group_reporting
```