# LXC "claude" (105) – Remote Control

- Proxmox: `https://192.168.1.200:8006` (utilizador `root@pam`)
- LXC: **105 (claude)**, Debian 13
- Sessão na app Claude: **claude-gleaming-stearns** (Remote Control)

## Quando a sessão fica em "Conectando"

Normalmente o login do Claude expirou (por exemplo, depois de mudar a senha da Google).

No **Terminal do Mac** (não na consola web do Proxmox, que não deixa colar), escreve **um comando de cada vez** e espera pelo prompt antes do seguinte:

```
ssh root@192.168.1.200
```
(senha do Proxmox; o prompt fica `root@pve`)

```
pct enter 105
```
(o prompt fica `root@claude`; não precisa da senha do LXC)

```
claude auth login
```
1. Escolher **Claude account with subscription** (não a Console / API).
2. Cmd+duplo-clique no link azul, clicar em **Authorize** e depois em **Copiar código**.
3. Voltar ao Terminal, Cmd+V e Enter. Deve aparecer `Login successful`.
4. Usar sempre o código do link **mais recente**. Um código de um login anterior dá "code wasn't accepted".

Depois, reiniciar o serviço:
```
systemctl restart claude-rc
```

## Religar a conversa "Cação code"

A conversa está na pasta `/homeassistant` do LXC 105. Dentro do LXC (`root@claude`):

```
cd /homeassistant && claude --resume 94251ee9-e0bc-4c55-af07-f017fdd9b92d
```
- Se perguntar "trust this folder": carregar **↓** até **Yes, I trust this folder** e só depois Enter. Enter direto escolhe "No, exit".
- Já dentro do Claude: `/remote-control` e Enter, e depois **Continue**.
- Não fechar essa janela com Ctrl+C. Se o terminal fechar, a sessão continua no tmux.

## Arranque automático

Serviço systemd `/etc/systemd/system/claude-rc.service`: corre `claude remote-control` numa sessão tmux chamada `rc` sempre que o LXC arranca.

- Estado: `systemctl status claude-rc`
- Ver a sessão: `tmux attach -t rc` (sair sem parar: Ctrl+B e depois D)
- Parar / arrancar: `systemctl stop claude-rc` / `systemctl start claude-rc`

## Notas

- `/login` só funciona **dentro** do Claude. No bash usa-se `claude auth login`.
- Os avisos `setlocale: ... No such file or directory` são inofensivos.
