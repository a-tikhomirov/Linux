task:
Представим, что ты делишь свою компьютер с другими ребятами, которые плохо знают Linux. Они подключаются к твоей машине по ssh, причем используют Терминал другой unix машины или Putty, не вводя пароль в настройках. Уже пятый раз тебе звонят и спрашивают, почему при вводе пароля символы не печатаются. Тебе это надоело и ты хочешь сделать так, чтобы при подключении по ssh ДО ввода пароля пользователю выводилось сообщение о том, что символы, которые он вводит не будут отображаться:

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


