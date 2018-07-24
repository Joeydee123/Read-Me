The data Science Toolkit 
	Amazon EC stands for “Amazon Elastic compute cloud”, it provides users with computing capacity in the Amazon Web Services (AWS) cloud while allowing developers access to Amazon’s hard drive in allowance to develop and deploy their applications. It can be used to launch any numbers of servers, configure security and networks, and manage storage. EC2 allows users to customize settings, so you’re paying for exactly what your using or to accommodate the transition of your business and personal needs. AWS Operating System is virtual computing environments that preconfigured templates for your instances, known as Amazon Machine Images (AMIs), package the bits you need for your server(including operating systems and any additional software, different configurations of CPU, memory, storage and networking capacity for your instances, known as instance types , include secure login information for your instances key pair storage volumes for temporary data/persistent storage. You will use Amazon EC to host your Jupyter notebooks and the data they store/collect. 
	 
Jupyter notebook security concerns of someone gaining access of your Jupyter notebook server and stealing /locking you out of your page is where we begin developing our Jupyter page.  You must first generate SSH keys. SSH keys serve as a means of identification to an SSH server. SSH Keys allow you to be authenticated by the server without having to send a password to the network as well as offer additional security.  They are generated in twos: a private key (known only to you) and the public key (which is used to connect and can be shared freely). Once an SSH server has your public key on file it, it will utilize it in sending you an encrypted message to gain access:  which must be met with the appropriate response which only the key will be able to understand. 
	The first thing we are going to do is set up SSH Key pair: we will do this utilizing BASH. Bash is essentially a shell, is a type of command line program that contains a simple, text-based user interface, enabling us to access all of an operating system’s services. For Mac users (such as myself) you can locate Bash by pressing the Command key +  space bar, which will call on the “Spotlight Search”,  you’ll then type in terminal , and click enter.  Which will pull up BASH. You’ll then want to type in the following code into the command line.
shh-keygen
Bash will request you allocate where the files should be stored. Click “enter” to have them stored in the default location. Press “enter” a second time to bypass entering a passphrase and avoid the annoyance of having to continuously renter it. 

 
Bash will save the two files under.ssh/id_rsa. and .ssh/id_rsa.pub in their default location. Next input the following code into the command line.
cat ~/.ssh/id_rsa.pub 
this will display the content of the file (in our case our public key) Make sure to copy the text generate onto your clipboard.  Then you want to return back to your AWS account. Go to your EC23 dashboard
 

Click on “Key Pairs” tab.

 

Input your public key pair by pasting the text you previously saved onto the area labeled “Public key contents”. You then need to name your key pair, you want to provide it with a practical name according to when and why they were generated, this will help you for future use (ex.2018_Aug_Datascience) Once you hit import, the key pair will be available for use on your dashboard. 
We are then going to configure a security group. Security groups serve as a firewall that monitors your instance’s traffic. You can customize the rules that govern its ingoing and outgoing traffic. We can do this by returning to the dashboard and selecting the  “Security Groups” tab, then on the upper left select the “Create Security Group” tab.
 
After naming the group, we are going to need to be able to do two things, connect to our system via SSH and HTTP. We are going to add those rules types, then under “Source”,  click on the drop down and  specify the rules are to be allowed “anywhere”. SSH port range should be assigned to 22, HTTP to 80 and HTTPS to 443. Once we have configured our security group we can launch our Jupyter server. 




Now we are going to click “Launch Instance”, to customize the instance we will be placing our software on.
 

AWS platform is easily navigated and allows users to swiftly generate and customized instances that efficiently meets their applications needs in 7 steps
1.	Choosing AMI ( Amazon Machine Image), in other words choosing a server, object oriented program. We are going to choose Ubuntu server 16.04.  It is a very popular server and becoming familiar with the software will later prove an advantage. 
2.	Choose Instances Type, we are going to use the default selection t2 micro. 
3.	Configure instance Detail, for our purposes can be skipped, but can be used to set spot instances
4.	 Add Storage, according to our instance we are allotted 30 GIB of space. You can change it to best meet your needs. 
5.	 Add Tags, for our purposes this step can be skipped.
6.	 Customize Security Groups, this is where you select the security group you previously created 
7.	Review and Launch.






Once you review your instance, click launch. AWS will ask you to select a key pair, you want to click on the drop down and “choose existing key pair”, then choose the pair we previously. 
 

Finally acknowledge the fact that you have access to the selected key pair by clicking the small box. The go ahead and click the “Launch Instances” tab. This will launch your instance, if you return to your dashboard and select “Instances” you can see it coming online. Scroll down to the description tab and copy the public IP address. You will need the IP address to connect to our running instance using SSH.


Next you want to return back to your BASH shell and enter the code 

ssh ubuntu@<ip> 

// unbunto is the user created on the instance for the ubuntu AIM. We are going to utilize that user and the IP address we generated on AWS. BASH will ask if you agree to the new host attempting to access the page, once you agree to the “new host” by inputting “yes” you will be connected to the remote server. 

Next, we want to install Docker on our AWS system using SSH. Docker is a software containerization platform that allows the user to accesses different applications across the hybrid cloud. It allows independence between applications, infrastructures and for potential collaboration between different models. We will utilize docker to manage our Jupyter Notebook. We will do this by entering the following code into the BASH command line
Curl -sSL https://get.docker.com | sh 
// this tells the computer to download the shell script for docker on your shell 
Sudo usermod -aG docker ubuntu
//This line adds our user into the ubuntu group

In order for this to take affect we need to log out using the code

exit 

And then log back in using the code

ssh ubuntu@<ip> 

You can then check that everything was correctly installed by  entering the following code and verifying the version of Docker that was just installed

docker -v

Finally was are ready to launch our Jupyter Notebook. Renter the following code into BASH

ssh ubuntu@<ip> 

Once we connect to the server we are going to pull the Jupyter image that we want to use to manage out notebook. We will do this by inputting 

 docker pull jupyter/datascience-notebook

(verifying with $ docker images)

 docker run -v /home/ubuntu:/home/jovyan -p 80:8888 -d dsnb

Docker will then return a long string, which is the container id. 

docker exec <containerid> jupyter notebook list 

The code will generate a token needed to access the page. First pull up your browser, input the public IP address into the address bar followed by :443. Then past the tokin  into the space labeled “password or  tokin” and login into your Jupyter notebook.


Diagram of Over all System

![](https://uclax.online/user/joeydee123/view/Screen%20Shot%202018-07-21%20at%209.42.57%20PM.png)





A detailed budget of the costs of running a Jupyter Data Science Notebook Server for three months using at least three different kinds of EC 2 instances.
Running Jupyter Data Science Notebook Server is fairly priced. Amazon Web Services offers a  twelve month free trial period,  “AWS Free Tier Details” which  includes 25GB of storage, 25 units of reading capacity and 25 units of writing capacity. We host our Data Science Notebook Servers from AWS for three months taking advantage of their trial period. All our Jupyter notebooks will used the free tier eligible Ubuntu 16.04 LTS (HVM), SSD Volume Type ami-5e8bb23b. server 


The first notebook will be the biggest, and we will allow the instance 12 DG of data, t’s instance type will be the general purpose t2.micro free tier eligible, purchase Spot instance of .05, coming to 35$ a month and 108$ for its three month duration of the server.


The second instance will be allowed 8 GB of data , it’s instance type will be the general purpose t2.micro free tier eligible. urchase Spot instance of .03, coming to 21$ a month and 65$ for its three month duration of the server.


The third instance will utilize the remaining 5 GB of data, it’s instance type will be the general purpose t2.micro free tier eligible, and 







