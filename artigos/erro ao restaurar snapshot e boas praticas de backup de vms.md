# VirtualBox / VMware: Erro ao restaurar Snapshot e boas práticas de backup de VMs

> **Alerta de Segurança:** Desligue completamente a MV e faça uma cópia manual da pasta do disco virtual antes de alterar arquivos de configurações ou snapshots.

## 1. Quando Aplicar (Sintoma)

Quando a VM apresentar falha ao usar um snapshot e exibir mensagens de erro, arquivos corrompidos ou quando precisa garantir a integridade dos dados antes de uma atualização importante.

## 2. Ferramentas/Componentes Necessários

* **Hardware / Físico:** Espaço livre em disco (HD/SSD) suficiente para armazenar clones completos e memória RAM adequada.
* **Software:** VirtualBox ou VMware e gerenciador de arquivos da máquina hospedeira.

## 3. Passo a Passo Rápido

1. Feche totalmente o VirtualBox ou VMware e verifique no gerenciador de tarefas se não há nenhum processo em aberto.
2. Localize a pasta da VM no computador hospedeiro e copie os arquivos principais para um diretório externo de backup.
3. Abra o gerenciador de mídia virtual do programa, verifique se há snapshots com avisos de erro e remova apenas os vínculos corrompidos.
4. Para backups seguros a longo prazo, evite depender exclusivamente de snapshots; utilize a função **Exportar Appliance (OVA/OVF)** ou **Clonar VM (Clone Completo)** com a máquina desligada.

   ```bash
   VBoxManage list vms
   ```
5. **Teste de Validação:** Inicie a máquina virtual restaurada ou clonada e confirme se o sistema operacional abrirá normalmente sem alertas de disco ou perda de arquivos.

linha de erro linha de erro