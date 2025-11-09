1. Oracle Ed-Analytics - Sprint 2

1.1. Sobre o Projeto

O Oracle Ed-Analytics é uma plataforma de análise comportamental desenhada para combater as altas taxas de evasão em plataformas de Ensino a Distância (EAD).

Enquanto métricas tradicionais (como visualizações e cliques) são superficiais, a nossa solução foca-se na Internet of Behavior (IoB) para entender como o aluno estuda.

Nesta Sprint 2, o nosso objetivo foi mover o projeto do conceito para a realidade, construindo e validando o pipeline de dados de ponta a ponta na nuvem da Oracle.

1.2. Arquitetura Funcional (Sprint 2)

O protótipo funcional validado nesta entrega consiste no seguinte fluxo de dados:

[Imagem de um diagrama de arquitetura simples: MQTT -> Node-RED -> APEX API -> Autonomous DB -> APEX Dashboard]

Simulador IoT (MQTTX): Um cliente MQTT simula um evento de comportamento do aluno (ex: pausou_video) e publica-o num tópico (ed-analytics/eventos).

Broker MQTT (Nuvem): Um broker público (broker.hivemq.com) recebe e distribui a mensagem.

OCI Compute (Node-RED): Uma VM na OCI, a correr Node-RED, está subscrita ao tópico MQTT. Ao receber a mensagem, ela processa o JSON.

APEX API (ORDS): O Node-RED envia os dados através de um POST HTTP para um endpoint RESTful (API) que criámos no APEX.

Autonomous Database: A API (criada com PL/SQL) recebe os dados e executa um INSERT na tabela ADMIN.ALUNO_EVENTOS.

APEX Dashboard: A nossa aplicação em APEX lê diretamente esta tabela e exibe os eventos em tempo real.

1.3. Ferramentas Oracle Utilizadas

Autonomous Database: Armazenamento central dos eventos.

Oracle APEX: Utilizado para duas funções críticas:

Front-end: Criação do "Log de Eventos" (o dashboard).

Back-end: Criação da API RESTful (via ORDS) para receber os dados do Node-RED.

OCI Compute (VM): VM Oracle Linux para hospedar o Node-RED.

OCI Networking (VCN): Configuração de VCN, Sub-redes e Security Lists (Firewall) para permitir a comunicação segura entre a VM e o APEX.

1.4. Como Testar o Protótipo

Dashboard: Aceda à aplicação "Dashboard EAD" no APEX.

Fluxo: Certifique-se que o servidor Node-RED está a correr na VM da OCI.

Simulador: Use um cliente MQTT (como o MQTTX) e conecte-se a broker.hivemq.com.

Publicar: Envie uma mensagem JSON para o tópico ed-analytics/eventos com o seguinte formato:

{
  "aluno": "nome_do_aluno",
  "acao": "acao_do_aluno",
  "aula": "id_da_aula"
}


Verificar: Atualize o dashboard no APEX. O novo evento deve aparecer na tabela.

1.5. Próximos Passos (Sprints 3 & 4)

Com o pipeline de dados validado, os próximos passos são:

Evoluir o Dashboard: Substituir o log por gráficos e KPIs reais.

Implementar a IA: Usar o serviço OCI Generative AI para ler os dados da tabela e gerar os insights automáticos que foram propostos na Sprint 1.
