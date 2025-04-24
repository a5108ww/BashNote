

# 建立資源群組
az group create --name ShilvainTest
# ResourceGroup='ShilvainTest'

# 建立App Service Plan
az appservice plan create --resource-group ShilvainTest --name ShilvainAppPlan --sku F1

# 刪除App Service Plan
az appservice plan delete --name ASP-ShilvainTest-a2f9 --resource-group ShilvainTest

# 通用變數
ResourceGroup='ShilvainTest'
AppServicePlan='ShilvainAppPlan'
VNet='ShilvainVNet01'
NSG='ShilvainNsg'
SubNet='ShilvainSubNet01'
NetCard='ShilvainNetCard01'
PublicIP='ShilvainPublicIP01'
VMName1='ShilvainVM01'

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
az vm create -n $VMName1 -g $ResourceGroup --image microsoftsqlserver:sql2014sp3-ws2012r2:sqldev:latest --size Standard_B1ms --os-disk-size-gb 128 --authentication-type password --admin-username shilvain --admin-password 1qaz2wsx#EDC4rfv --nic $NetCard