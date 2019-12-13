**task:**
Представим, что ты делишь свою компьютер с другими ребятами, которые плохо знают Linux. Они подключаются к твоей машине по ssh, причем используют Терминал другой unix машины или Putty, не вводя пароль в настройках. Уже пятый раз тебе звонят и спрашивают, почему при вводе пароля символы не печатаются. Тебе это надоело и ты хочешь сделать так, чтобы **при подключении по ssh** **ДО ввода пароля** пользователю выводилось сообщение о том, что символы, которые он вводит не будут отображаться:

```console
********************************************************************
*                        <<< WARNING >>>                           *
*==================================================================*
* This ubuntu machine is solely for the use of authorized users.   *
* Hopefully, you know what you're up to. Just remember:            *
*                                                                  *
*      Do not allow children to play in the dishwasher!!!          *
*                                                                  *
* Oh, sorry... I meant be aware:                                   *
*                                                                  *
*        You will not see symbols while typing your password.      *
*  That's ok - they are still typed. We care about your security.  *
********************************************************************
```

- А теперь сделай так, чтобы для всех пользователей это сообщение выводилось, а для пользователя **root** это сообщение не выводилось.

**1:**
Создаем файл с баннером:

```ShellSession
~ $ sudo vim /etc/ssh/sshd-banner
```

**2:**
Меняем конфигурацию **sshd**:

```ShellSession
~ $ sudo vim /etc/ssh/sshd_config
```

```vim
#Banner none
Banner /etc/ssh/sshd-banner
```

Перезапускаем **sshd**:	

```ShellSession
~ $ service sshd restart
```

**3:**
Проверяем:

```Console
login as: cbpi
********************************************************************
*                        <<< WARNING >>>                           *
*==================================================================*
* This ubuntu machine is solely for the use of authorized users.   *
* Hopefully, you know what you're up to. Just remember:            *
*                                                                  *
*      Do not allow children to play in the dishwasher!!!          *
*                                                                  *
* Oh, sorry... I meant be aware:                                   *
*                                                                  *
*        You will not see symbols while typing your password.      *
*  That's ok - they are still typed. We care about your security.  *
********************************************************************
cbpi@192.168.41.251's password:
```

**4:**
Отключаем баннер для пользователя **root**:

Меняем конфигурацию **sshd**:

```ShellSession
~ $ sudo vim /etc/ssh/sshd_config
```

```vim
# Example of overriding settings on a per-user basis
#Match User anoncvs
#       X11Forwarding no
#       AllowTcpForwarding no
#       PermitTTY no
#       ForceCommand cvs server
Match user root
        Banner none
```

Перезапускаем **sshd**:	

```ShellSession
~ $ service sshd restart
```

**5:**
Проверяем:

```Console
login as: root
root@192.168.41.251's password:
```

```Console
login as: cbpi
********************************************************************
*                        <<< WARNING >>>                           *
*==================================================================*
* This ubuntu machine is solely for the use of authorized users.   *
* Hopefully, you know what you're up to. Just remember:            *
*                                                                  *
*      Do not allow children to play in the dishwasher!!!          *
*                                                                  *
* Oh, sorry... I meant be aware:                                   *
*                                                                  *
*        You will not see symbols while typing your password.      *
*  That's ok - they are still typed. We care about your security.  *
********************************************************************
cbpi@192.168.41.251's password:
```
