
# Instalação de Ferramentas de Integração: Resolução e Área de Transferência em Máquinas Virtuais

> **Alerta de Segurança:** Antes de instalar ou atualizar ferramentas de integração, crie um snapshot da VM ou faça backup. Utilize somente pacotes oficiais compatíveis com a versão do hipervisor e do sistema operacional convidado. Não habilite a área de transferência compartilhada em VMs não confiáveis, pois ela permite transferência de dados entre host e convidado.

## 1. Quando Aplicar (Sintoma)

A resolução da tela permanece travada em uma dimensão fixa, não acompanha o redimensionamento da janela ou apresenta baixa qualidade de vídeo.  
A área de transferência não permite copiar e colar textos entre o computador hospedeiro (host) e a máquina virtual (guest).

## 2. Ferramentas/Componentes Necessários

* **Hardware / Físico:** Computador hospedeiro com virtualização habilitada (Intel VT-x ou AMD-V), memória RAM e espaço em disco suficientes.
* **Software:**
  * VMware Workstation / VMware Fusion / ESXi.
  * Oracle VirtualBox.
  * Microsoft Hyper-V.
  * Windows 10/11 ou distribuição Linux suportada.
  * VMware Tools ou Open VM Tools.
  * VirtualBox Guest Additions.
  * Hyper-V Integration Services e, quando necessário, XRDP.
  * Acesso administrativo (Administrador ou root).

## 3. Passo a Passo Rápido

1. **Identifique o hipervisor e prepare a VM:**
   - Identifique se a máquina virtual utiliza VMware, VirtualBox ou Hyper-V.
   - Atualize o sistema operacional convidado.
   - Crie um snapshot antes de realizar alterações.
   - Verifique se a VM possui memória de vídeo suficiente e se a aceleração gráfica está configurada adequadamente.

2. **Instale os pacotes de integração correspondentes ao hipervisor:**

   **A) VMware Workstation / Fusion / ESXi**

   **Windows:**
   - Inicie a máquina virtual e faça login com uma conta administrativa.
   - No menu do VMware, selecione `VM > Install VMware Tools` ou `Guest OS > Install VMware Tools`, conforme a versão.
   - Abra a unidade de DVD virtual no Windows.
   - Execute `setup.exe` como administrador.
   - Selecione a instalação Typical e conclua o assistente.
   - Reinicie a máquina virtual.

   **Linux (Ubuntu/Debian):**
   - Prefira instalar o pacote Open VM Tools pelos repositórios oficiais da distribuição.
   - Execute:

   ```bash
   sudo apt update
   sudo apt install -y open-vm-tools open-vm-tools-desktop
   sudo reboot
   ```

   Para distribuições Fedora/RHEL:

   ```bash
   sudo dnf install -y open-vm-tools
   sudo reboot
   ```

   O pacote `open-vm-tools-desktop` fornece componentes adicionais de integração gráfica em ambientes desktop compatíveis.

   **Configuração:**
   - Nas configurações da VM, habilite o redimensionamento automático da tela (Auto-fit Guest).
   - Verifique se as opções de copiar e colar (Copy and Paste) estão permitidas nas configurações de isolamento de convidado, quando disponíveis.
   - Reinicie a sessão gráfica se necessário.

   **B) Oracle VirtualBox**

   **Windows:**
   - Com a VM ligada, abra o menu `Devices > Insert Guest Additions CD Image`.
   - No Windows convidado, abra a unidade de CD virtual.
   - Execute `VBoxWindowsAdditions.exe` como administrador.
   - Siga o assistente de instalação e reinicie a VM.

   **Linux (Ubuntu/Debian):**

   Instale as dependências para compilação dos módulos do kernel:

   ```bash
   sudo apt update
   sudo apt install -y build-essential dkms linux-headers-$(uname -r)
   ```

   Depois:
   - Selecione `Devices > Insert Guest Additions CD Image` no VirtualBox.
   - Execute o instalador presente na mídia virtual:

   ```bash
   sudo mkdir -p /mnt/cdrom
   sudo mount /dev/cdrom /mnt/cdrom
   sudo sh /mnt/cdrom/VBoxLinuxAdditions.run
   sudo reboot
   ```

   **Configuração:**
   - No menu `Devices > Shared Clipboard`, selecione `Bidirectional`.
   - Em `Devices > Auto-resize Guest Display`, habilite o redimensionamento automático.
   - Se necessário, desligue a VM e verifique se a memória de vídeo está configurada adequadamente nas opções de vídeo.

   **C) Microsoft Hyper-V**

   **Windows:**
   - Nas versões atuais do Windows, os serviços de integração já fazem parte do sistema operacional.
   - No Hyper-V Manager, abra as configurações da VM.
   - Verifique os serviços de integração disponíveis.
   - Habilite o Enhanced Session Mode no host e permita seu uso nas configurações do servidor Hyper-V.
   - Conecte-se à VM usando o Enhanced Session Mode.
   - Na janela de conexão, habilite a área de transferência e os dispositivos locais desejados.

   Para verificar os serviços no Windows convidado:

   ```powershell
   Get-Service -Name vmic*
   ```

   **Linux:**
   - As distribuições Linux modernas possuem drivers Hyper-V integrados ao kernel.
   - Atualize o kernel e instale os componentes auxiliares disponíveis na distribuição.

   Exemplo para Ubuntu:

   ```bash
   sudo apt update
   sudo apt install -y linux-virtual hyperv-daemons
   sudo reboot
   ```

   - Para resolução dinâmica e área de transferência em uma sessão gráfica, configure o XRDP e os componentes de sessão aprimorada compatíveis com a distribuição.
   - O Enhanced Session Mode do VMConnect com compartilhamento de dispositivos é oficialmente disponibilizado para convidados Windows. Não presuma que a instalação dos serviços Hyper-V, isoladamente, habilitará clipboard bidirecional em Linux.

   **Observação:** No Hyper-V, a cópia de arquivos por Guest Service Interface é diferente da área de transferência. Para convidados Linux, verifique os recursos efetivamente suportados pela distribuição e pelo método de conexão utilizado.

3. **Teste de Validação:**

   Após concluir a instalação e reiniciar a VM:

   - **Resolução:** Redimensione a janela do hipervisor e verifique se a resolução do sistema convidado acompanha as dimensões da janela.
   - **Área de transferência:** Copie um texto simples do host e cole no guest. Depois, realize o procedimento inverso.
   - **Drivers:** Verifique se os serviços e módulos de integração estão ativos.
   - **Persistência:** Reinicie a VM e repita os testes para confirmar que as configurações foram mantidas.

   Comandos adicionais para diagnóstico:

   **VMware Linux:**
   ```bash
   systemctl status open-vm-tools
   vmware-toolbox-cmd -v
   ```

   **VirtualBox Linux:**
   ```bash
   lsmod | grep vbox
   sudo rcvboxadd status-kernel
   sudo rcvboxadd status-user
   ```

   **Hyper-V Linux:**
   ```bash
   lsmod | grep hv_
   systemctl status hv-kvp-daemon
   ```

   **Hyper-V Windows (PowerShell):**
   ```powershell
   Get-Service -Name vmic*
   ```

   **Resultado esperado:** A VM deve acompanhar o redimensionamento da janela, apresentar resolução adequada e permitir copiar e colar textos entre host e guest quando o recurso for suportado e estiver habilitado.

## Referências Técnicas

- VMware Tools: https://knowledge.broadcom.com/external/article/315363/installing-and-upgrading-vmware-tools-in.html
- VirtualBox Guest Additions: https://docs.oracle.com/en/virtualization/virtualbox/7.2/user/guestadditions.html
- Hyper-V Enhanced Session Mode: https://learn.microsoft.com/en-us/windows-server/virtualization/hyper-v/enhanced-session-mode
- Hyper-V Integration Services: https://learn.microsoft.com/en-us/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services
