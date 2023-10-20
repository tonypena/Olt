# Olt
scrpt basicos de olt


========= BUSCAR ONUs PARA PROVISIONAR ========
OLT-DM4610-AMAZON# show interface gpon discovered-onus
------ Resultado do comando abaixo ------

 Chassis / Slot / Port  Serial Number
 ---------------------  -------------
       1/1/2            DACM9131CC5C

========= BUSCANDO UM ID PARA A ONU ==========
OLT-DM4610-AMAZON# show interface gpon 1/1/2 onu ?
Possible completions:
  |   Output modifiers
  ---
  0  1  2  3  <cr>
------ O resultado acima diz que a proxima ONU deverá ser a ONU de número 4 --------
                   
========= Entrando no modo configuração =========
OLT-DM4610-AMAZON# config
Entering configuration mode terminal
OLT-DM4610-AMAZON(config)# interface gpon 1/1/2 onu 4        		#entrando na porta gpon e atribuindo um numero pra ela
OLT-DM4610-AMAZON(config-gpon-onu-4)# name TUP-ONU-001      		#dando um nome para o equipamento
OLT-DM4610-AMAZON(config-gpon-onu-4)# serial-number DACM9131CC5C	#inserindo o serial do equipamento
OLT-DM4610-AMAZON(config-gpon-onu-4)# line-profile CAMERAS		#chamando o profile padrão
OLT-DM4610-AMAZON(config-gpon-onu-4)# ethernet 1			#habilitando a porta
OLT-DM4610-AMAZON(config-ethernet-1)# native vlan vlan-id 110		#usado apenas para ONU Bridge
OLT-DM4610-AMAZON(config-ethernet-1)# top				#voltando para o inicio de configuração
OLT-DM4610-AMAZON(config)# service-port new				#criando um novo serviço
OLT-DM4610-AMAZON(config-service-port-10)# gpon 1/1/2 onu 4 gem 1 match vlan vlan-id 110 action vlan replace vlan-id 110 description TUP-ONU-001
OLT-DM4610-AMAZON(config-service-port-10)# commit			#salvando as configurações
Commit complete.
OLT-DM4610-AMAZON(config-service-port-10)# end				#voltando para o diretório raiz
