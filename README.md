
ebs_test
========

## Description

Test EBS-related functionality of Eucalyptus

## Procedure

1. Create between 1 and 3 keys using ec2-add-keypair
2. Create 1 managed ip using ec2-llocate-address
3. Create 1-3 groups using ec2-add-group
4. Create ingress rules for icmp and ssh using ec2-authorize-group for each group
5. Get available EMIs on the box, choose a random one
6. Get the keypairs, choose a random one
7. Get admin addresses choose a random one
8. Get groups and choose a random one that is not default
9. Run 1 instance with those parameters
10. Check for running test instance, loop TRIES times waiting for INTERVAL seconds in between checks
11. [FAIL] The instance never started
12. Get the instance key pair
13. Create a volume of size VOLSIZE GB
14. Check that the volume is ready for TRIES times waiting for INTERVAL seconds
15. Attach volume to the instance with the name /dev/vda
16. Make sure device is ready
17. mkfs.ext2 for /dev/vda then mount the volume to the instance at /mnt
18. Write a file to the volume at /mnt/mark
19. Cat the file and ensure it is the same as when we made it
20. Unmount volume
21. Detach volume from instance
22. Create a snapshot of the volume, check that it is completed
23. Create a volume with the snapshot, check that it completed
24. Ensure the volume is ready
25. Attach the new volume to the instance and check that it is available at /dev/vdb
26. Mount the volume to /mnt
27. Check that the file we created is still there and accurate
28. Unmount the volume
29. Detach the volume
30. Delete all volumes and snapshots



<hr><hr><hr>

# Eucalyptus Testunit Framework

Eucalyptus Testunit Framework is designed to run a list of test scripts written by Eucalyptus developers.



## How to Set Up Testunit Environment

On **Ubuntu** Linux Distribution,

### 1. UPDATE THE IMAGE

<code>
apt-get -y update
</code>

### 2. BE SURE THAT THE CLOCK IS IN SYNC

<code>
apt-get -y install ntp
</code>

<code>
date
</code>

### 3. INSTALL DEPENDENCIES
<note>
YOUR TESTUNIT **MIGHT NOT** NEED ALL THE PACKAGES BELOW; CHECK THE TESTUNIT DESCRIPTION.
</note>

<code>
apt-get -y install git-core bzr gcc make ruby libopenssl-ruby curl rubygems swig help2man libssl-dev python-dev libright-aws-ruby nfs-common openjdk-6-jdk zip libdigest-hmac-perl libio-pty-perl libnet-ssh-perl euca2ools
</code>

### 4. CLONE test_share DIRECTORY FOR TESTUNIT
<note>
YOUR TESTUNIT **MIGHT NOT** NEED test_share DIRECTORY. CHECK THE TESTUNIT DESCRIPTION.
</note>

<code>
git clone git://github.com/eucalyptus-qa/test_share.git
</code>

### 4.1. CREATE /home/test-server/test_share DIRECTORY AND LINK IT TO THE CLONED test_share

<code>
mkdir -p /home/test-server
</code>

<code>
ln -s ~/test_share/ /home/test-server/.
</code>

### 5. CLONE TESTUNIT OF YOUR CHOICE

<code>
git clone git://github.com/eucalyptus-qa/**testunit_of_your_choice**
</code>

### 6. CHANGE DIRECTORY

<code>
cd ./**testunit_of_your_choice**
</code>

### 7. CREATE 2b_tested.lst FILE in ./input DIRECTORY

<code>
vim ./input/2b_tested.lst
</code>

### 7.1. TEMPLATE OF 2b_tested.lst, SEPARATED BY TAB

<sample>
192.168.51.85	CENTOS	6.3	64	REPO	[CC00 UI CLC SC00 WS]

192.168.51.86	CENTOS	6.3	64	REPO	[NC00]
</sample>

### 7.2. BE SURE THAT YOUR MACHINE's id_rsa.pub KEY IS INCLUDED THE CLC's authorized_keys LIST

ON **YOUR TEST MACHINE**:

<code>
cat ~/.ssh/id_rsa.pub
</code>

ON **CLC MACHINE**:

<code>
vim ~/.ssh/authorized_keys
</code>

### 8. RUN THE TEST

<code>
./run_test.pl **testunit_of_your_choice.conf**
</code>


## How to Examine the Test Result

### 1. GO TO THE artifacts DIRECTORY

<code>
cd ./artifacts
</code>

### 2. CHECK OUT THE RESULT FILES

<code>
ls -l
</code>


## How to Rerun the Testunit

### 1. CLEAN UP THE ARTIFACTS

<code>
./cleanup_test.pl
</code>

### 2. RERUN THE TEST

<code>
./run_test.pl **testunit_of_your_choice.conf**
</code>


