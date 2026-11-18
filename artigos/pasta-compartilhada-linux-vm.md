# Resolvendo inacessibilidade de Pastas Compartilhadas entre máquinas virtuais da VMWare e hipervisores no Windows 11 
## 1. Quando aplicar
- As soluções propostas são para usuários não conseguem acessar as pastas compartilhadas entre VM's do VMWare e a máquina hispervisora do Windows 11
## 2. Possíveis causas 
1. **As configurações de rede que permitem compartilhamento avançado pelo Windows não foram realizadas corretamente**
1. **A opção "descoberta de rede privada" não está ligada no Windows**
1. **A configuração do compartilhamento avançado não foi configurada corretamente na pasta a ser compartilhada**
1. **A opção "Shared Folders" não está ligada na seção "Options" da máquina virtual**
1. **A pasta a ser acessada não foi montada no S.O. do Linux** 
## 3. Passo a Passo para cada causa
### Primeiro caso e Segundo caso
- Acesse configurações → rede e internet → configurações avanvaçadas de rede → configurações de compartilhamento avançadas 
- Há três opções com configurações similares (Redes Privadas/Públicas/Domínio/Todas)
- Habilite a configuração "Compartilhamento de arquivos e impressoras" **(para o primeiro caso)**
- Habilite a configuração "Descoberta de Rede" **(para o segundo caso)**
>** Alerta de Segurança: Não ative a descoberta de rede de tipos de rede que você não vai utilizar**
### Terceiro caso
- Acesse as propriedades avançadas da pasta (alt+Enter) → Compartilhamento → Compartilhamento Avançado → Adicione a máquina virtual ao grupo de usuários com todas as permissões
>** Alerta de Segurança: Não coloque para que o usuário "Todos" tenham acesso completo à qualquer pasta enquanto estiver na rede pública**
### Quarto caso
- Acesse aba de edição de configurações da máquina desejada → "Options" → "Shared Folders" → Escolha entre a 2ª ou 3ª opção → clique em "Add" → escolha o caminho da pasta em "Browse" → Nomeie como quiser → Marque a opção "Enable this share" → "Finish"   
### Quinto caso
- Instale o VMWare Tools com: 
```bash
sudo apt update && sudo apt install open-vm-tools open-vm-tools-desktop
```
- Com o terminal Bash aberto digite:
```bash
vmhgfs-fuse .host:/ /mnt/hgfs -o allow-other 
```
jkasdfhkljasdhfjksdhkj fhasl