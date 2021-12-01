

theme: jekyll-theme-slate

# Beginning of Description of Wireguard VPN (using Docker) Project 3
To begin, we shall first SSH into the created Droplet from the local host terminal;
1) enter in the following command [ssh root@your.droplet.address.here] #root is the default username
2) once completed, it will then ask for you password, adn then should be able to ssh into droplet server

Then begins the process of installing Docker itself.
1) Run the following commands:
  mkdir -p ~/wireguard/
  mkdir -p ~/wireguard/config/
  nano ~/wireguard/docker-compose.yml
2) Copy and paste the following (with necessary changes beneath)
  version: '3.8'
services:
  wireguard:
    container_name: wireguard
    image: linuxserver/wireguard
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Hong_Kong
      - SERVERURL=1.2.3.4
      - SERVERPORT=51820
      - PEERS=pc1,pc2,phone1
      - PEERDNS=auto
      - INTERNAL_SUBNET=10.0.0.0
    ports:
      - 51820:51820/udp
    volumes:
      - type: bind
        source: ./config/
        target: /config/
      - type: bind
        source: /lib/modules
        target: /lib/modules
    restart: always
    cap_add:
      - NET_ADMIN
      - SYS_MODULE
    sysctls:
      - net.ipv4.conf.all.src_valid_mark=1
      
  Once the above has been copied and pasted, you must then change the following above:
    1) Change the timezone (TZ) (find yours on wikipedia) (America/Chicago for central time zone)
    2) Change SERVERUL (ip address) which you can find on the DigitalOcean dashboard
    3) Change PEERS; with PEERS=3 will generate peer_1 through peer_3;
       PEERS=pc1,pc2,phone1 will produce peer_pc1, peer_pc2, and peer_phone1
       
       
Wireguard can be started with [cd  ~/wireguard/] and then [docker-compose up -d] # This would build the server (Should finish when "Creating wireguard  ...done")

There are two options in getting a before/after screenshot, your phone or laptop/computer. # We need to do both versions for screenshots
For phones: Produce an execution log and QR codes of the VPN using the code [docker-compose logs -f wireguard];
            Make sure that you have the app itself on your phone in order to scan the QR code after clicking the (+) and choosing the scan from QR code option;
            Once that is done, post screenshots based on IPLeak.net of before and after turning on the VPN; (Capture screenshot of VPN tunnel as active)
            
For laptops: First find the config files using the command [~/wireguard/configs/{username}] and search for the .conf file to d/l (download) or copy to laptop;
             Next, do the before/after with the VPN and screenshot the results from IPLeak.net and capture a screenshot of VPN tunnel being active);
             (Be sure to take these versions side by side with VPN tunnel proof)

For this case: cd into the ./config folder, ls its contents, cd into any avaliable "peer" folders you have made, ls again, and use the [cat] command for its .config file.
               Copy the contents afterward and paste it into a .txt file (set to "all files") make sure its name is avaliable with .conf. Download the app and choose said
               .conf file that you have created and click activate.
               
               
FROM THIS POINT ON SCREENSHOTS (PHONE AND LAPTOP VERSIONS):

Laptop:
![Screenshot (2)](https://user-images.githubusercontent.com/90016656/143978335-03d3b0f5-4321-45fd-8677-1b7ce4d112a6.png)

Phone:
![Merged_document](https://user-images.githubusercontent.com/90016656/143978865-e5bd1234-1478-4a5b-9b36-862e1813a378.png)
               
  # For the laptop version, I had issues; Instead of the original command, I instead found the .conf file using ls ~/wireguard/config and using the command
  # xdg-open ~/wireguard/config/wg0.conf to get the file contents.