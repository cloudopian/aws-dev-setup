There are two ways you can use these CloudFormation templates. Namely simple & advanced. 

### How create a simple development environment
Use this when you just want a simple network setup and you have no plan to experiment with multiple network layouts. 

- First deploy the CloudFormation template **DevSetup-Network.template**. You don’t need to provide any parameters for it. It will create a VPC with CIDAR range 10.0.0.0/16 and four subnets. This includes 2 public subnets 10.0.1.0/24 & 10.0.2.0/24 & two private subnets 10.0.3.0/24 & 10.0.4.0/24. It will also setup the correct route tables, network connectivity and an EC2 instance profile for your development VM to use. This CloudFormation does not include any billed resources.  

- Then deploy the CloudFormation template **DevSetup-WindowsDevMachine.template**. 
    - For the parameter *DevMachineInstanceTypeParameter*

      Select an EC2 instance size you want for your development EC2 instance. The on-demand instance pricing is given [here]( https://aws.amazon.com/ec2/pricing/on-demand/)

    - For the parameter *DevSetupNetworkStackName*

      Use the CloudFormation stack name you used when you deploy **DevSetup-Network.template**.

    - For the parameter *MainPassword* 

      Give the password you want for remote login to your VM. The username is *Administrator*.  

Once the instance starts, you can remote login with the username and password.
When the instance starts it will install set of useful software [Chocolatey,Google chrome,7Zip,Notepad++,VSCode,AWS CLI]. You can easily customize what get installed by changing the section *Resources>DevMachine>Metadata* in **DevSetup-WindowsDevMachine.template**

[Here is a demo](http://www.youtube.com/watch?v=bHaQ7X45jdA) on how to use these templates.
 

### How create an advanced development environment
Use this to create multiple network layouts so that you can test things such as [peering connections](https://docs.aws.amazon.com/vpc/latest/peering/what-is-vpc-peering.html), [cross VPC connections](https://docs.aws.amazon.com/whitepapers/latest/aws-vpc-connectivity-options/amazon-vpc-to-amazon-vpc-connectivity-options.html) etc. 

-	First deploy the CloudFormation template **DevSetup-Network-Advance.template**. This allows you to customize the network layout.  For the parameter *Id*, use a value between 0 and 10. This allows you to create non-overlapping sample networks with different VPC CIDAR range. 

    For example, if you use **Id=1** it will create a VPC with cidar 10.**1**.0.0/16 and if you use the **Id=5**, it will create a VPC with cidar range 10.**5**.0.0/16 & so on. It will also create two public subnets 10.**{Id}**.1.0/24 & 10.**{Id}**.2.0/24  & two private subnets 10.**{Id}**.3.0/24 & 10.**{Id}**.4.0/24 with correct routing. It will also provision an EC2 instance profile to use with development machine. This CloudFormation does not include any billed resources.  

-	Then deploy the CloudFormation **DevSetup-WindowsDevMachine-Advanced.template**
    - For the parameter *DevMachineInstanceTypeParameter*
    
      Select an EC2 instance size you want for your development EC2 instance. The on-demand instance pricing is given [here]( https://aws.amazon.com/ec2/pricing/on-demand/).  

    - For parameter *DevSetupNetworkStackName* 
      
      Use the CloudFormation stack name you gave when you deploy **DevSetup-Network-Advance.template**.
    
    - For the parameter *MainPassword*
      
      Give the password you want for remote login to your VM. The username is *Administrator*.  

Once the instance starts, you can remote login with the username and password.
When the instance starts it will install set of useful software [Chocolatey,Google chrome,7Zip,Notepad++,VSCode,AWS CLI & some VS code extensions]. You can easily customize what get installed by changing the section *Resources>DevMachine>Metadata* in  **DevSetup-WindowsDevMachine-Advanced.template**

 






