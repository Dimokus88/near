# Near Stake Wars на Akash Network!
___

***Near*** проводит эвент ***Stake Wars*** , целью которого является создания валидаторов, которые будут создавать небольшие снимки сети - чанки. Здесь ***Near*** публикует задания. По итогам эвента будут отобраны валидаторы для основной сети.

___

#### Содержание:
1. [Подготовка](https://github.com/Dimokus88/near/blob/main/Guide_RU.md#подгтовка).
2. [Создаем и регистрируем валидатора](https://github.com/Dimokus88/near/blob/main/Guide_RU.md#создаем-и-регистрируем-валидатора).

___

## Подготовка

### Установка Akashlytics

Инструкция по установке и настройке ***Akashlytics*** [доступна по ссылке](https://github.com/Dimokus88/guides/blob/main/Akashlytics/RU-guide.md).

### Создание аккаунта (кошелька) в сети Near - Shardnet.

Инструкция по созданию аккаунта (кошелька) [в сети Near - Shardnet](https://github.com/Dimokus88/near/blob/main/Shardnet_wallet.md#создание-аккаунта-кошелька-в-сети-near---shardnet).
  
___

### Создаем и регистрируем валидатора.

* Разворачиваем контейнер используя ***этот [deploy.yml](https://github.com/Dimokus88/near/blob/main/deploy.yml)***, незабудьте указать пароль пользователя ***root***. 

> P.S. Если у вас уже есть ***validator_key.json***, то просто [вставьте прямую ссылку](https://user-images.githubusercontent.com/23629420/180153979-181daa3c-2e68-43d8-b3ab-622de8f9ff00.png) на скачивание к переменной ```$link_key``` и запустите развертывание, ***больше ничего делать не прийдется!*** 

* Дожидаемся этого сообщения во вкладке `LOGS`-`LOGS`:
  
<p align="center"><img src="https://user-images.githubusercontent.com/23629420/180058368-5233ff77-984b-46ec-afdb-d9c394097e68.png" width=60% </p>

* Подкючаемся по `ssh` к работающему контейнеру по параметрам указаным в `LEASES`:

<p align="center"><img src="https://user-images.githubusercontent.com/23629420/180058824-bd322297-39a1-4111-bcde-7b2d49dfc50e.png" width=50% </p>   

Пользователь ***root**, пароль - тот что вы указали в ***манифесте (deploy.yml)***:

<p align="center"><img src="https://user-images.githubusercontent.com/23629420/180059094-c278dd3a-f7c6-4eb1-8df3-493429af91ed.png" width=50% </p>   

На данном этапе нода развернута и ***уже начала синхронизироваться***, вы можете посмотреть логи ноды командой:
```
tail -30 /root/.near/nohup.err
```
* Для регистрации валидатора вводим команду:
```
near login
``` 
и отвечаем на запрос `y` (yes) , в ответ будет выдана ссылка:

<p align="center"><img src="https://user-images.githubusercontent.com/23629420/180059646-235c2fd5-805b-4288-9e42-771586a92355.png" width=60% </p>   

* Перейдите по ссылке в браузере, где открыт аккаунт в тестовой сети ***near*** и подтвердите связь ноды и аккаунта:

<p align="center"><img src="https://user-images.githubusercontent.com/23629420/180060700-55aaa8b6-f43f-4d20-8877-aeae354356ce.png" width=40% </p>   

<p align="center"><img src="https://user-images.githubusercontent.com/23629420/180060751-7ca65aa2-e39e-4321-8f10-842b0467913b.png" width=40% </p>   

<p align="center"><img src="https://user-images.githubusercontent.com/23629420/180060903-3b5204bb-c2f5-43ba-84c9-c6f5557c9013.png" width=40% </p>   

Когда в ответ увидите такое сообщение - все в порядке, вернитесь в терминал:

<p align="center"><img src="https://user-images.githubusercontent.com/23629420/180062810-e94ccfe3-7c4e-4b4b-b456-f80a59006f30.png" width=50% </p>   

* Введите имя вашего аккаунта, в ответ получите сообщение о том что аккаунт привязан:

<p align="center"><img src="https://user-images.githubusercontent.com/23629420/180063083-d6dd3e1a-af55-42a1-a7ec-9d288a88d368.png" width=60% </p>   

* Теперь необходимо создать ключи валидатора (***validator_key.json***), делаем команду где ***ХХ - ваш account id***, для меня команда будет выглядеть так: `near generate-key akash_user.factory.shardnet.near`:
```
near generate-key xx.factory.shardnet.near
```
  
В ответ получите сообщение о том что пара ключей создана:

<p align="center"><img src="https://user-images.githubusercontent.com/23629420/180063876-ad6dc292-46e5-45cd-a74f-6d193a19ecd6.png" width=80% </p>   

* Скопируйте файл кошелька в папку `.near`, где ***ХХ - ваш account id***. :
```
cp ~/.near-credentials/shardnet/ХХ.shardnet.near.json ~/.near/validator_key.json
```

  * Внесите изменения файл `validator_key.json`:
```
nano ~/.near/validator_key.json
```

В поле `account_id` добавьте `.factory`, что бы получилось `ХХ.factory.shardnet.near.json` где ***ХХ - ваш account id***.

Измените название поля с `private_key` на `secret_key`.

* Сохраните изменения: нажмите `ctrl+x`, затем `y` и `enter`. Создайте валидатора командой:
```
near call factory.shardnet.near create_staking_pool '{"staking_pool_id": "<pool id>", "owner_id": "<accountId>", "stake_public_key": "<public key>", "reward_fee_fraction": {"numerator": 5, "denominator": 100}, "code_hash":"DD428g9eqLL8fWUxv8QSpVFzyHi1Qd16P8ephYCTmMSZ"}' --accountId="<accountId>" --amount=30 --gas=300000000000000
```

где: 
 
`<pool id>` - ваше имя аккаунта (у меня в примере `akash_user`).
  
`<accountId>` - полный account Id(у меня в примере `akash_user.shardnet.near`).
  
`<public key>` - публичный ключ из `validator_key.json`.
    
<p align="center"><img src="https://user-images.githubusercontent.com/23629420/180076787-2d0d845c-921b-4ac8-8701-5cdb8e940975.png" width=80% </p>   
    
На успешную команду вы получите ответс ссылкой в explorer, [***пример***](https://explorer.shardnet.near.org/transactions/V4a6mJzP71PGBKmuqFcwwySFgdXWiLkux4UFfTtzezw):
    
<p align="center"><img src="https://user-images.githubusercontent.com/23629420/180077001-34055547-683a-41d4-aff7-e834e89398d7.png" width=60% </p>   
    
* Чтобы заделегировать дополнительные токены нужно выполнить команду
```
near call <staking_pool_id> deposit_and_stake --amount <amount> --accountId <accountId> --gas=300000000000000
```

где: 
    
`<staking_pool_id>` - ваше имя валидатора (у меня в примере `akash_user.factory.shardnet.near`).
  
`<accountId>` - полный account Id (у меня в примере `akash_user.shardnet.near`).
  
`<amount>` - количество токенов для делегации.

В ответ так же поступит ссылка в explorer [***пример***](https://explorer.shardnet.near.org/transactions/CRUdCmQ7tAh8vFk7QkDRhu8iRH9SVtP6RrGKhgEnRE5Q):

<p align="center"><img src="https://user-images.githubusercontent.com/23629420/180078082-97aed13b-d040-4521-9c3d-423f8b9b7ab7.png" width=60% </p>
    
***Валидатор создан***, незабудьте у себя сохранить локально файл `validator_key.json` ,это можно сделать скопировав вывод команды:
```
cat ~/.near/validator_key.json | jq
```
  
[К началу](https://github.com/Dimokus88/near/blob/main/README.md#%D1%81%D0%BE%D0%B4%D0%B5%D1%80%D0%B6%D0%B0%D0%BD%D0%B8%D0%B5).

___
