網路資源
https://blog.gcp.expert/gcp-running-windows-server-failover-clustering-1/

決定我要的國家跟地區
國家 : asia-east1
地區 : asia-east1-a

--建立Net
gcloud compute networks create gcpnet --subnet-mode custom

--建立SubNet
gcloud compute networks subnets create gcpsubnet --network gcpnet --region asia-east1 --range 192.168.0.0/16

--刪除SubNet
gcloud compute networks subnets delete [NAME]

--建立防火牆規則

(任何IP都能夠連VM的22 port)
gcloud compute firewall-rules create gcpallow22port --network gcpnet --allow tcp:22 --source-ranges 0.0.0.0/0

gcloud compute firewall-rules create gcpforsteam --network gcpnet --allow tcp:2456-2458,udp:2456-2458 --source-ranges 0.0.0.0/0

gcloud compute firewall-rules create allow-ssh --network gcpnet --allow tcp:22,tcp:3389 --source-ranges [YOUR_IPv4_ADDRESS]

--刪除防火牆規則
gcloud compute firewall-rules delete allow-all-subnet

--建立外部IP
gcloud compute addresses create publicip1 --region=asia-east1

--釋放IP
gcloud compute addresses delete publicip1 --region=asia-east1

--建立VM

//windowsVM(不確定可不可行)
gcloud compute instances create shilvainvm --zone asia-east1-a --machine-type=e2-custom-2-4096 --image-project windows-cloud --image-family windows-2016 --boot-disk-size 64 --boot-disk-type pd-balanced --can-ip-forward --private-network-ip 192.168.1.4 --address=35.226.102.93 --network gcpnet --subnet gcpsubnet

//Linux
gcloud compute instances create shilvainvm --zone asia-east1-a --machine-type=e2-custom-2-4096 --image-project centos-cloud --image-family centos-7 --boot-disk-size 64 --boot-disk-type pd-balanced --can-ip-forward --private-network-ip 192.168.1.4 --address=34.81.14.101 --network gcpnet --subnet gcpsubnet

gcloud compute instances create shilvainvm --machine-type=e2-custom-2-4096 --image-project centos-cloud --image-family centos-7 --boot-disk-size 64 --boot-disk-type pd-balanced --can-ip-forward --private-network-ip 192.168.1.4 --address=34.80.106.90  --network gcpnet --subnet gcpsubnet

防火牆的允許 HTTPS 流量 記得勾起來

* address 所在的Zone 要跟VM 一樣
* 名稱不允許大寫

--建立計畫
gcloud compute instance-groups unmanaged create shilvaingroup --zone=asia-east1
gcloud compute instance-groups unmanaged add-instances shilvaingroup --instances shilvainVM --zone asia-east1

--檢視各項資源
gcloud compute firewall-rules list (防火牆規則)
gcloud compute addresses list(外部IP)

//設定當前資源所在的區域
gcloud config set compute/zone us-central1-b

//查詢各資源的清單
gcloud compute [RESOURCE]+s(es) list
範例1 : 要查詢帳號中的Network
指令 => gcloud compute Networks list

範例2 : 要查詢ˇ帳號中有幾台VM
指令 => gcloud compute instances list
gcloud compute addresses list

