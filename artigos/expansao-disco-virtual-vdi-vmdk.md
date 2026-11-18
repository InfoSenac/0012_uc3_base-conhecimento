# [Expansao-disco-virtual-vdi-vmdk]

> **Alerta de Segurança:** [Ex.: Desligue a VM antes de criar um novo disco virtual]

## 1. Quando Aplicar (Sintoma)
Necessidade desse procedimento seu disco virtual esta cheio e voce precisa de mais arquivos ou documentos,so que nao tem espaço.

## 2. Ferramentas/Componentes Necessários
* **Hardware**[VMware,Virtual-Box,Disco-virtual] 
* **Físico:** [CPU,memoria-ram-disco-fisico:SSD-ou-HD]
* **Software:**[Clonezilla-Gparted]

## 3. Passo a Passo Rápido
1. Desliga sua VM,cria um disco virtual maior que seu disco atual.
2. Formata seu disco novo tanto no gparted ou no proprio sistema.
3. Inicializa sua VM com a iso (clonezilla) localiza seu disco novo e formatado.
4. Faz o backups do disco para seu disco novo.

**Teste de Validação**
Verificar se no clonezilla concluiu o backup.
Inicializa a VM (Linux ou Windows) certifica se o novo disco com o backups esta com partição.
 
 Linha de teste com erro