🧠 1. Conceito

EC2 = Servidores virtuais (VMs) na nuvem da AWS.

Você pode escolher:

🧱 Tipo de instância → define CPU, memória, rede. (ex: t2.micro, t3.medium, etc.)

🌍 Região e Zona de Disponibilidade → define onde fisicamente sua máquina estará.

🧰 Sistema Operacional → Linux, Windows Server, AMIs customizadas.

⚙️ 2. Ciclo de Vida de uma Instância
Estado	Descrição	Ações comuns
Pending	Está sendo inicializada	Aguardar
Running	Instância em execução	Conectar, fazer deploy
Stopping	Encerrando em andamento	Aguardar
Stopped	Instância desligada, mas preserva dados do disco (EBS)	Start novamente
Terminated	Instância removida permanentemente	Não pode mais recuperar

Windows: acesso via RDP com chave para decifrar senha do administrador.

🔐 Boas práticas

Nunca expor porta 22 (SSH) para “0.0.0.0/0” permanentemente.

Usar IP fixo da sua rede ou AWS Systems Manager (Session Manager) para acesso seguro sem SSH aberto.

🛡️ 4. Security Groups

Funcionam como firewalls virtuais controlando tráfego de entrada e saída.

Regras baseadas em:

Portas (ex: 22 para SSH, 80/443 para HTTP/HTTPS)

Protocolo (TCP/UDP/ICMP)

IP/CIDR de origem ou destino.

Insight

Manter mínimo necessário de portas abertas é fundamental para segurança.
Exemplo: liberar 22 apenas para seu IP fixo, e 80/443 para o público.

🧱 5. Elastic IP

EC2 recebe IP dinâmico por padrão → muda se reiniciar a instância.

Para IP fixo → usar Elastic IP (EIP).

Você aloca um EIP e associa à instância.

⚠️ Se EIP estiver alocado mas não associado, a AWS cobra.

💾 6. Armazenamento (EBS)

O “HD” da instância é um EBS Volume.

Você pode:

Criar snapshots (backups incrementais)

Aumentar tamanho do volume sem perder dados

Anexar volumes extras para dados, logs etc.

💡 Insight: Automatizar snapshots regulares é prática comum para recuperação de desastres.

🔄 7. Gerenciamento no Painel vs CLI
📊 Painel AWS (Console)

Ideal para iniciantes e visualização rápida.

Permite criar, iniciar, parar, terminar e monitorar instâncias com interface gráfica.

💻 AWS CLI

Útil para automação, scripts e CI/CD.

Exemplos:

# Listar instâncias
aws ec2 describe-instances

# Parar instância
aws ec2 stop-instances --instance-ids i-1234567890abcdef0

# Iniciar instância
aws ec2 start-instances --instance-ids i-1234567890abcdef0


⚡ Insight: Automatizar tarefas com CLI ou Terraform reduz erros manuais.

🚀 8. Deploy de Aplicações

Passos típicos ao usar EC2:

Criar a instância com AMI Linux (ex: Amazon Linux 2)

Abrir portas necessárias no Security Group

Conectar via SSH

Instalar dependências (ex: Nginx, .NET, Python, Node)

Fazer deploy (ex: via Git, scp, CI/CD)

Configurar firewall e serviços como Systemd para rodar em background

✨ Insight: EC2 funciona muito bem com:

Route 53 (DNS personalizado)

Load Balancer (ALB) para distribuir tráfego

Auto Scaling Groups para escalar horizontalmente

💸 9. Custos

Cobrança por hora/segundo da instância + armazenamento EBS + IP elástico + tráfego.

Tipos de cobrança:

On-demand (padrão)

Spot Instances (mais baratas, mas podem ser interrompidas)

Reserved (contrato longo prazo, menor preço)

💡 Insight: Parar instâncias não usadas e limpar EIPs/volumes órfãos evita surpresas na fatura 💰

🧠 10. Boas Práticas Resumidas

🔐 Restringir SSH / usar Session Manager

📝 Usar tags para organização (ex: Name, Environment)

💾 Fazer snapshots automáticos

🌐 Usar Elastic IP ou Load Balancer se precisar de IP fixo

📈 Monitorar com CloudWatch (CPU, rede, disco)

🧹 Desligar ou excluir recursos não usados
