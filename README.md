# charge_game_releases
A repository for the releases of the private charge game repository.

The program has a client and server part, both of which can be downloaded from the releases section.

The server can be run locally or deployed to the cloud (for example Amazone EC2).
# Deploying to EC2
## Setup
1. Generate Key-Pair (to make it possible to ssh to EC2 Instance)
    - AWS EC2 Console -> Network & Security -> Key Pairs
    - Click 'Create key pair'
        - Provide name (e.g. unity-ec2-charge)
        - Use .pem
        - Press 'Create key pair', which should also download the file
    - Navigate to directory with key pair
        - Run `chmod 400 <key-pair-name>` 
        - e.g. `chmod 400 unity-ec2-charge`
2. Launch EC2 Instance
    - AWS EC2 Console - Press Launch Instance
    - **Application and OS Images (Amazon Machine Image)**
        - Default (Amazon Linux)
    - **Instance Type**
        - Default (t2.micro)
        - Ensure that this is "Free tier eligible"
    - **Key Pair (login)**
        - Select the key pair created earlier
        - e.g. `unity-ec2-charge`
    - **Network Settings**
        - Create security group
            - Allow SSH traffic from `My IP` (Not technically necessary, but is more secure)
            - Allow UDP connections from anywhere (Can also specify the port range)
    - **Configure storage**
        - Default
    - **Advanced details**
        - Default
    - Press "Launch Instance" when done with the above settings
3. Modify Unity project (optional)
    -  Copy Public IPv4 address from EC2 Instance
    - In Unity project, go to Initialization scene
    - Edit NetworkManager -> Tugboat (Component)
        - Change Client Address to EC2 Instance's IP address 
            - (Note that this will just change the default IP address, the client ui includes the option to change it back to "localhost")
4. Build Project
    - Dedicated Server - Linux
    - Linux Server Build because the EC2 Instance is a Linux Instance
5. Upload Build
    - Note: if running into problems with **Windows Powershell**, try **Git Bash** or **WSL**
    - Run 
        - `scp -r -i <.pem file> <build folder> ec2-user@<instance-ip>:~/.`
    - e.g. Where .pem and build folder are in same directory as where console was run from:
        - `scp -r -i unity-ec2-charge.pem Server_Linux ec2-user@##.###.##.###:~/.`
6. Make build executable
    - Copy "Public IPv4 DNS" from EC2 Instance
    - Connect to server via ssh
        - Run `ssh -i <pem> ec2-user@<ip or dns>`
        - e.g. `ssh -i unity-ec2-charge.pem ec2-user@ec2-##-###-##-###.ap-southeast-1.compute.amazonaws.com`
    - Navigate to folder with server build executable
    - Run `chmod +x <executable>`
        - e.g. `chmod +x Charge_Server.x86_64`

## Running the Server After Deployment
1. In AWS EC2 Console -> Instances
    - Copy "Public IPv4 DNS"
2. Connect via ssh
    - Run `ssh -i <pem> ec2-user@<ip or dns>`
    - e.g. `ssh -i unity-ec2-charge.pem ec2-user@ec2-##-###-##-###.ap-southeast-1.compute.amazonaws.com`
3. Navigate to folder with server build executable
4. Run `./<executable name>`
    - e.g. `./Charge_Server.x86_64`
