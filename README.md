# **README - Azure Secure Ubuntu VM Deployment**

## **📌 Visão Geral**  
Este projeto utiliza um **ARM Template (Azure Resource Manager)** para implantar uma **Máquina Virtual Ubuntu 24.04 LTS** no Azure com configurações de segurança avançadas, incluindo:  
- **Secure Boot** e **vTPM** (Trusted Platform Module virtualizado)  
- Autenticação via **SSH Key** (sem senha)  
- **Network Security Group (NSG)** personalizável  
- **Zonas de Disponibilidade** para alta resiliência  
- Configurações de **patch management** automático  
- Opções de **cleanup** (exclusão de discos e NICs quando a VM é removida)  

---

## **📋 Pré-requisitos**  
1. **Conta Azure** ativa com permissões para criar recursos.  
2. **Azure CLI** ou **PowerShell** instalado (para implantação).  
3. **Chave SSH pública** (para autenticação na VM).  
4. Conhecimento básico de **ARM Templates** e **Azure Networking**.  

---

## **🛠️ Recursos Implantados**  
| Recurso | Descrição |
|---------|-----------|
| **Virtual Machine** | Ubuntu 24.04 LTS com Secure Boot e vTPM |
| **Virtual Network (VNet)** | Rede virtual com sub-rede configurável |
| **Network Security Group (NSG)** | Regras de segurança personalizáveis |
| **Public IP Address** | IP público com suporte a zonas de disponibilidade |
| **Network Interface (NIC)** | Interface de rede com NSG e IP público |
| **Managed Disk (OS Disk)** | Disco gerenciado (SSD Premium/Standard) |

---

## **⚙️ Parâmetros Configuráveis**  

| Parâmetro | Descrição | Exemplo |
|-----------|-----------|---------|
| `location` | Região do Azure onde os recursos serão implantados | `eastus` |
| `virtualMachineName1` | Nome da VM | `my-secure-vm` |
| `virtualMachineSize` | Tamanho da VM | `Standard_B2s` |
| `adminUsername` | Usuário admin da VM | `azureuser` |
| `adminPublicKey` | Chave SSH pública | `ssh-rsa AAAAB3...` |
| `virtualNetworkName` | Nome da VNet | `my-vnet` |
| `addressPrefixes` | Bloco CIDR da VNet | `["10.0.0.0/16"]` |
| `subnets` | Configuração de sub-redes | `[{"name": "default", "addressPrefix": "10.0.1.0/24"}]` |
| `networkSecurityGroupRules` | Regras de NSG | `[{"name": "SSH", "priority": 100, ...}]` |
| `publicIpAddressSku` | SKU do IP Público | `Standard` |
| `securityType` | Tipo de segurança | `TrustedLaunch` |
| `secureBoot` | Habilita Secure Boot | `true` |
| `vTPM` | Habilita vTPM | `true` |
| `osDiskType` | Tipo de disco (SSD Premium/Standard) | `Premium_LRS` |
| `virtualMachine1Zone` | Zona de disponibilidade | `1` |

---

## **🚀 Como Implantar**  

### **1. Implantação via Azure CLI**  
```bash
az deployment group create \
  --resource-group <NomeDoResourceGroup> \
  --template-file azuredeploy.json \
  --parameters @azuredeploy.parameters.json
```

### **2. Implantação via PowerShell**  
```powershell
New-AzResourceGroupDeployment `
  -ResourceGroupName <NomeDoResourceGroup> `
  -TemplateFile azuredeploy.json `
  -TemplateParameterFile azuredeploy.parameters.json
```

---

## **🔐 Configurações de Segurança**  
- **SSH Key Authentication**: Apenas autenticação por chave SSH (senha desabilitada).  
- **Trusted Launch**:  
  - **Secure Boot**: Impede a execução de malware no boot.  
  - **vTPM**: Habilita criptografia baseada em hardware.  
- **Network Security Group (NSG)**: Regras personalizáveis para filtrar tráfego.  
- **Patch Management**:  
  - `assessmentMode`: Verifica atualizações periodicamente.  
  - `patchMode`: `ImageDefault` (usa patches da imagem base).  

---

## **🗑️ Gerenciamento de Recursos**  
- **Delete Options**:  
  - `osDiskDeleteOption`: Define se o disco é excluído quando a VM é removida.  
  - `nicDeleteOption`: Define se a NIC é excluída quando a VM é removida.  
  - `pipDeleteOption`: Define se o IP público é excluído quando a VM é removida.  

---

## **📤 Saídas (Outputs)**  
| Saída | Descrição |
|-------|-----------|
| `adminUsername` | Retorna o nome do usuário admin configurado |

---

## **📌 Observações Importantes**  
✅ **Ubuntu 24.04 LTS**: Imagem otimizada para workloads modernos.  
✅ **Zonas de Disponibilidade**: Melhora a resiliência contra falhas.  
✅ **Cleanup Automatizado**: Evita custos desnecessários após exclusão.  
⚠️ **Custo**: Verifique os preços dos recursos antes de implantar.  

---

## **📚 Documentação de Referência**  
- [Azure ARM Templates](https://docs.microsoft.com/azure/azure-resource-manager/templates/)  
- [Ubuntu 24.04 LTS no Azure](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/canonical.0001-com-ubuntu-server-jammy)  
- [Azure Trusted Launch](https://docs.microsoft.com/azure/virtual-machines/trusted-launch)  

---

## **📝 Licença**  
MIT License.  

---

**🎯 Pronto para implantar?**  
👉 Clone o repositório, ajuste os parâmetros e execute o deployment!  

```bash
git clone https://github.com/seu-usuario/azure-secure-ubuntu-vm.git
cd azure-secure-ubuntu-vm
az deployment group create --resource-group my-rg --template-file azuredeploy.json --parameters @params.json
```  
