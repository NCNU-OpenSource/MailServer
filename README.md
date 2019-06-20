
# MailServer
## 材料
### 安裝
1. postfix
2. mail
3. pop3 + imap
<pre>
  sudo apt-get install postfix    (mailserver)
  sudo apt-get install mailutils  (MUA)
  sudo apt-get install dovecot-pop3d  (通訊協定)
  sudo apt-get install dovecot-imapd  (通訊協定)
</code></pre>
### 確認

#### postfix : 
<pre>
  telnet localhost 25
</code></pre>
應出現：
<pre>
  Trying 127.0.0.1...
  Connected to localhost.
  Escape character is '^]'.
  220 lsa1072.com ESMTP Postfix (Ubuntu)
</code></pre>
#### pop3 : 
<pre>
  telnet localhost pop3
</code></pre>
<pre>
  Trying 127.0.0.1...
  Connected to localhost.
  Escape character is '^]'.
  +OK Dovecot ready.
</code></pre>
#### imap : 
<pre>
  telnet localhost imap
</code></pre>
<pre>
  Trying 127.0.0.1...
  Connected to localhost.
  Escape character is '^]'.
  * OK [CAPABILITY IMAP4rev1 LITERAL+ SASL-IR LOGIN-REFERRALS ID ENABLE STARTTLS AUTH=PLAIN] Dovecot ready
</code></pre>

## mailserver觀念

### MUA(Mail User Agent)
編寫信件或收取信件的平台/程式
### MTA(Mail Transfer Agent)
包含SMTP和Relay
將目的地不是本地端的信往外送（relay）
將目的地是本地端的信收進來
### MDA (Mail Delivery Agent)
決定送進來的信要不要收

## 防火牆設置
/.../.../.../.../.../.../iptables.rule 中
找到以下幾行 將註解拿掉
#### mailserver的port
iptables -A INPUT -p TCP -i $EXTIF --dport  25  --sport 1024:65534 -j ACCEPT
#### imap的port
iptables -A INPUT -p TCP -i $EXTIF --dport 143  --sport 1024:65534 -j ACCEPT
#### pop3的port
iptables -A INPUT -p TCP -i $EXTIF --dport 110  --sport 1024:65534 -j ACCEPT

## 修改main.cf的參數
每次改動之後都要 sudo postfix restart
位置:/etc/postfix/main.cf
找到 relayhost =
改成 relayhost = [ms1.hinet.net]

