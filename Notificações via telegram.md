```
URL:
 Method: POST / URL: https://api.telegram.org/bot{{ secrets.BOT_TOKEN }}/sendMessage?chat_id={{ secrets.CHAT_ID }}

Headers:
 Content-Type application/json

Body:
 {
"text": "⚠️Proxmox Notificação⚠️

 Titulo: {{ title }}
 Mensagem: {{ message }}

 Severidade: {{ severity }}
 Momento:{{ timestamp }}"
 }

Secrets:
 BOT_TOKEN SEU_TOKEN_TELEGRAM
 CHAT_ID  SEU_CHATID_TELEGRAM
```
