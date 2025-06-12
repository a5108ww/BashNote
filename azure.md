
# 通用變數
ResourceGroup='ShilvainProd'
AppServicePlan='ShilvainProdAppPlan-AP'
AppServicePlan='ShilvainProdAppPlan-API'
NSG='ShilvainProdNsg'
VNet='ShilvainProdVNet'
SubNet='ShilvainProdSubNet'
NetCard='ShilvainNetCard01'
PublicIP='ShilvainPublicIP01'
VMName1='ShilvainVM01'
AppServiceName='shilvainweb'

# 建立資源群組
az group create --name ShilvainTest --location eastasia
az group create --name ShilvainProd --location eastasia

# 建立App Service Plan
az appservice plan create --resource-group $ResourceGroup --name $AppServicePlan --location eastasia --sku F1 --hyper-v
az appservice plan create --resource-group $ResourceGroup --name $AppServicePlan --location eastasia --sku D1 --hyper-v
(--hyper-v = Window容器)

# 刪除App Service Plan
az appservice plan delete --name ASP-ShilvainTest-a2f9 --resource-group ShilvainTest

# 建立App Service
//V4.8
az webapp create -g $ResourceGroup -p $AppServicePlan --n $AppServiceName --runtime "ASPNET:V4.8" --deployment-local-git

//dotnet8
az webapp create -g $ResourceGroup -p $AppServicePlan --n $AppServiceName --runtime "Dotnet:8" --deployment-local-git

# Vnet

## 建立
az network vnet create -n $VNet -g $ResourceGroup --address-prefix 192.168.0.0/16  --location eastasia

## 刪除
az network vnet delete -n $VNet -g $ResourceGroup

# 防火牆規則

## 建立
az network nsg create -n $NSG -g $ResourceGroup --location eastasia

## 刪除
az network nsg delete -g $ResourceGroup -n $NSG

# 建立SubNet

## 建立
az network vnet subnet create -n $SubNet -g $ResourceGroup --vnet-name $VNet --address-prefix 192.168.1.0/24 --network-security-group $NSG

## 刪除
az network vnet subnet delete -n $SubNet -g $ResourceGroup --vnet-name $VNet

# 建立網卡

## 建立
az network nic create -n $NetCard -g $ResourceGroup --vnet-name $VNet --subnet $SubNet --public-ip-address $PublicIP

## 刪除
az network nic delete -n $NetCard -g $ResourceGroup

# Public IP

## 建立
az network public-ip create -n $PublicIP -g $ResourceGroup --sku standard --allocation-method static

## 刪除
az network public-ip delete -n $PublicIP -g $ResourceGroup

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

# 列出目前支援那些執行階段
az webapp list-runtimes

"linux": [
    "DOTNETCORE:9.0",
    "DOTNETCORE:8.0",
    "NODE:22-lts",
    "NODE:20-lts",
    "NODE:18-lts",
    "PYTHON:3.13",
    "PYTHON:3.12",
    "PYTHON:3.11",
    "PYTHON:3.10",
    "PYTHON:3.9",
    "PHP:8.4",
    "PHP:8.3",
    "PHP:8.2",
    "PHP:8.1",
    "JAVA:21-java21",
    "JAVA:17-java17",
    "JAVA:11-java11",
    "JAVA:8-jre8",
    "JBOSSEAP:8-java17",
    "JBOSSEAP:8-java11",
    "JBOSSEAP:7-java17",
    "JBOSSEAP:7-java11",
    "JBOSSEAP:7-java8",
    "TOMCAT:11.0-java21",
    "TOMCAT:11.0-java17",
    "TOMCAT:11.0-java11",
    "TOMCAT:10.1-java21",
    "TOMCAT:10.1-java17",
    "TOMCAT:10.1-java11",
    "TOMCAT:10.0-java17",
    "TOMCAT:10.0-java11",
    "TOMCAT:10.0-jre8",
    "TOMCAT:9.0-java21",
    "TOMCAT:9.0-java17",
    "TOMCAT:9.0-java11",
    "TOMCAT:9.0-jre8",
    "TOMCAT:8.5-java11",
    "TOMCAT:8.5-jre8"
  ],
  "windows": [
    "dotnet:9",
    "dotnet:8",
    "ASPNET:V4.8",
    "ASPNET:V3.5",
    "NODE:22LTS",
    "NODE:20LTS",
    "NODE:18LTS",
    "JAVA:21",
    "JAVA:17",
    "JAVA:11",
    "JAVA:8",
    "TOMCAT:11.0-java21",
    "TOMCAT:11.0-java17",
    "TOMCAT:11.0-java11",
    "TOMCAT:11.0-java8",
    "TOMCAT:10.1-java21",
    "TOMCAT:10.1-java17",
    "TOMCAT:10.1-java11",
    "TOMCAT:10.1-java8",
    "TOMCAT:10.0-java21",
    "TOMCAT:10.0-java17",
    "TOMCAT:10.0-java11",
    "TOMCAT:10.0-java8",
    "TOMCAT:9.0-java21",
    "TOMCAT:9.0-java17",
    "TOMCAT:9.0-java11",
    "TOMCAT:9.0-java8",
    "TOMCAT:8.5-java21",
    "TOMCAT:8.5-java17",
    "TOMCAT:8.5-java11",
    "TOMCAT:8.5-java8"
  ]

# 列出目前有哪些地區
az account list-locations -o table

DisplayName               Name                 RegionalDisplayName
------------------------  -------------------  -------------------------------------
East US                   eastus               (US) East US
South Central US          southcentralus       (US) South Central US
West US 2                 westus2              (US) West US 2
West US 3                 westus3              (US) West US 3
Australia East            australiaeast        (Asia Pacific) Australia East
Southeast Asia            southeastasia        (Asia Pacific) Southeast Asia
North Europe              northeurope          (Europe) North Europe
Sweden Central            swedencentral        (Europe) Sweden Central
UK South                  uksouth              (Europe) UK South
West Europe               westeurope           (Europe) West Europe
Central US                centralus            (US) Central US
South Africa North        southafricanorth     (Africa) South Africa North
Central India             centralindia         (Asia Pacific) Central India
East Asia                 eastasia             (Asia Pacific) East Asia
Indonesia Central         indonesiacentral     (Asia Pacific) Indonesia Central
Japan East                japaneast            (Asia Pacific) Japan East
Japan West                japanwest            (Asia Pacific) Japan West
Korea Central             koreacentral         (Asia Pacific) Korea Central
Malaysia West             malaysiawest         (Asia Pacific) Malaysia West
New Zealand North         newzealandnorth      (Asia Pacific) New Zealand North
Canada Central            canadacentral        (Canada) Canada Central
France Central            francecentral        (Europe) France Central
Germany West Central      germanywestcentral   (Europe) Germany West Central
Italy North               italynorth           (Europe) Italy North
Norway East               norwayeast           (Europe) Norway East
Poland Central            polandcentral        (Europe) Poland Central
Spain Central             spaincentral         (Europe) Spain Central
Switzerland North         switzerlandnorth     (Europe) Switzerland North
Mexico Central            mexicocentral        (Mexico) Mexico Central
UAE North                 uaenorth             (Middle East) UAE North
Brazil South              brazilsouth          (South America) Brazil South
Chile Central             chilecentral         (South America) Chile Central
East US 2 EUAP            eastus2euap          (US) East US 2 EUAP
Israel Central            israelcentral        (Middle East) Israel Central
Qatar Central             qatarcentral         (Middle East) Qatar Central
Central US (Stage)        centralusstage       (US) Central US (Stage)
East US (Stage)           eastusstage          (US) East US (Stage)
East US 2 (Stage)         eastus2stage         (US) East US 2 (Stage)
North Central US (Stage)  northcentralusstage  (US) North Central US (Stage)
South Central US (Stage)  southcentralusstage  (US) South Central US (Stage)
West US (Stage)           westusstage          (US) West US (Stage)
West US 2 (Stage)         westus2stage         (US) West US 2 (Stage)
Asia                      asia                 Asia
Asia Pacific              asiapacific          Asia Pacific
Australia                 australia            Australia
Brazil                    brazil               Brazil
Canada                    canada               Canada
Europe                    europe               Europe
France                    france               France
Germany                   germany              Germany
Global                    global               Global
India                     india                India
Indonesia                 indonesia            Indonesia
Israel                    israel               Israel
Italy                     italy                Italy
Japan                     japan                Japan
Korea                     korea                Korea
Malaysia                  malaysia             Malaysia
Mexico                    mexico               Mexico
New Zealand               newzealand           New Zealand
Norway                    norway               Norway
Poland                    poland               Poland
Qatar                     qatar                Qatar
Singapore                 singapore            Singapore
South Africa              southafrica          South Africa
Spain                     spain                Spain
Sweden                    sweden               Sweden
Switzerland               switzerland          Switzerland
Taiwan                    taiwan               Taiwan
United Arab Emirates      uae                  United Arab Emirates
United Kingdom            uk                   United Kingdom
United States             unitedstates         United States
United States EUAP        unitedstateseuap     United States EUAP
East Asia (Stage)         eastasiastage        (Asia Pacific) East Asia (Stage)
Southeast Asia (Stage)    southeastasiastage   (Asia Pacific) Southeast Asia (Stage)
Brazil US                 brazilus             (South America) Brazil US
East US 2                 eastus2              (US) East US 2
East US STG               eastusstg            (US) East US STG
North Central US          northcentralus       (US) North Central US
West US                   westus               (US) West US
Jio India West            jioindiawest         (Asia Pacific) Jio India West
Central US EUAP           centraluseuap        (US) Central US EUAP
South Central US STG      southcentralusstg    (US) South Central US STG
West Central US           westcentralus        (US) West Central US
South Africa West         southafricawest      (Africa) South Africa West
Australia Central         australiacentral     (Asia Pacific) Australia Central
Australia Central 2       australiacentral2    (Asia Pacific) Australia Central 2
Australia Southeast       australiasoutheast   (Asia Pacific) Australia Southeast
Jio India Central         jioindiacentral      (Asia Pacific) Jio India Central
Korea South               koreasouth           (Asia Pacific) Korea South
South India               southindia           (Asia Pacific) South India
West India                westindia            (Asia Pacific) West India
Canada East               canadaeast           (Canada) Canada East
France South              francesouth          (Europe) France South
Germany North             germanynorth         (Europe) Germany North
Norway West               norwaywest           (Europe) Norway West
Switzerland West          switzerlandwest      (Europe) Switzerland West
UK West                   ukwest               (Europe) UK West
UAE Central               uaecentral           (Middle East) UAE Central
Brazil Southeast          brazilsoutheast      (South America) Brazil Southeast