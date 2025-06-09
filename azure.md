
# 通用變數
ResourceGroup='ShilvainProd'
AppServicePlan='ShilvainProdAppPlan'
NSG='ShilvainProdNsg'
VNet='ShilvainProdVNet'
SubNet='ShilvainProdSubNet'
NetCard='ShilvainNetCard01'
PublicIP='ShilvainPublicIP01'
VMName1='ShilvainVM01'

# 建立資源群組
az group create --name ShilvainTest
# ResourceGroup='ShilvainTest'

# 建立App Service Plan
az appservice plan create --resource-group ShilvainTest --name ShilvainAppPlan --sku F1

# 刪除App Service Plan
az appservice plan delete --name ASP-ShilvainTest-a2f9 --resource-group ShilvainTest

# 建立App Service
//V4.8
az webapp create -g $ResourceGroup -p $AppServicePlan --n webShilvain01 --runtime "aspnet|V4.8" --deployment-local-git

//core 3.1
az webapp create -g $ResourceGroup -p $AppServicePlan --n webShilvain02 --runtime "DOTNETCORE|3.1" --deployment-local-git

# 建立Vnet
az network vnet create -n $VNet -g $ResourceGroup --address-prefix 192.168.0.0/16

# 建立防火牆規則
az network nsg create -n $NSG -g $ResourceGroup

# 建立SubNet
az network vnet subnet create -n $SubNet -g $ResourceGroup --vnet-name $VNet --address-prefix 192.168.1.0/24 --network-security-group $NSG

# 建立網卡
az network nic create -n $NetCard -g $ResourceGroup --vnet-name $VNet --subnet $SubNet --public-ip-address $PublicIP

# 建立Public IP
az network public-ip create -n $PublicIP -g $ResourceGroup --sku standard --allocation-method static

# 建立VM

### 必要參數

-n：虛擬機器的名字
-g：資源群組名字

### 常用選擇性參數

--image：操作系統映像的名稱。發行者識別碼:產品識別碼:方案識別碼(MarketPlace)。範例：microsoftsqlserver:sql2014sp3-ws2012r2:sqldev:latest、microsoftsqlserver:sql2019-ws2022:sqldev:latest
--size：要建立的 VM 等級。範例：Standard_B1s、Standard_B1ms。
--os-disk-size-gb：要建立的 OS 磁碟大小以 GB 為單位。
--authentication-type：VM驗證類型，接受的值: all, password, ssh。
--admin-username：VM 的用戶名稱。
--admin-password：VM 的用戶密碼。
--nics：要連結至 VM 之現有 NIC 的名稱或識別碼。 

##### 範例1

az vm create -n $VMName1 -g $ResourceGroup --image microsoftsqlserver:sql2014sp3-ws2012r2:sqldev:latest --size Standard_B1s --os-disk-size-gb 64 --authentication-type password --admin-username shilvain --admin-password 1qaz2wsx#EDC4rfv --nics $NetCard

##### 範例2

az vm create -n $VMName1 -g $ResourceGroup --image microsoftsqlserver:sql2019-ws2022:sqldev:latest --size Standard_B1s --os-disk-size-gb 64 --authentication-type password --admin-username shilvain --admin-password 1qaz2wsx#EDC4rfv --nics $NetCard