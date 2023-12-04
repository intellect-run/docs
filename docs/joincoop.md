Раздел в разработке
<!-- ---
title: Вступить в пайщики
---
После того, как вы получили статус верифицированного кооператива, вы можете регистрировать физические и юридические лица в свой кооператив. 


## Хранилища данных
Для хранения приватных данных используется IPFS - интерпланетная файловая система. Она работает по принципу torrent-ссылок на зашифрованную информацию. Таким образом, пользователи хранят свою приватную информацию в зашифрованном виде в локальных хранилищах и предоставляют доступ к зашифрованным копиям информации, которые могут быть расширофрованы только приватными ключами тех аккаунтов в блокчейне, которым был предоставлен доступ. 

### Модель приватных данных пользователя
Модель данных для хранения в приватном хранилище должна следовать стандартизированной структуре. На основе этой структуры данных в дальшейшем любой клиент сможет восстановить ваш электронный документ при наличии соответствующих прав доступа, определяющихся приватными ключами доступа к аккаунту.


!!! note "Стандарт модели"
    ```javascript
    new Schema({
      first_name: { type: String },       // Имя
      second_name: { type: String },      // Фамилия
      middle_name: { type: String },      // Отчество
      birthdate: { type: Date },          // Дата рождения
      addresses: [{
        address: { type: String },        // Адрес целиком
        postal_code: { type: String },    // Почтовый индекс
        country: { type: String },        // Страна
        region: { type: String },         // Регион
        city: { type: String },           // Город
        settlement: { type: String },     // Поселение
        street: { type: String },         // Улица
        block: { type: String },          // Блок или корпус
        house: { type: String },          // Номер дома
        floor: { type: String },          // Этаж
        flat: { type: String },           // Квартира
        room: { type: String },           // Номер комнаты
        is_active: { type: Boolean },     // Адрес активен?
        is_verified: { type: Boolean },   // Адрес верифицирован?
        address_type: { 
          type: Number, 
          enum: [0, 1, 2, 3, 4]           // Тип адреса: юридический, домашний и т.д.
        }
      }],
      phones: [{
        phone_number: { type: String },   // Номер телефона
        is_primary: { type: Boolean },    // Основной номер?
        is_active: { type: Boolean },     // Номер активен?
        is_telegram: { type: Boolean },   // Номер используется в Telegram?
        is_whatsapp: { type: Boolean },   // Номер используется в WhatsApp?
        is_viber: { type: Boolean },      // Номер используется в Viber?
        last_update: { type: Date },      // Дата последнего обновления
        phone_type: { 
          type: Number, 
          enum: [0, 1, 2, 3]              // Тип номера: домашний, рабочий, мобильный и т.д.
        }
      }],
      email: { type: String },            // Адрес электронной почты
      socials: [{
        soc_user: { type: String },          // Идентификатор пользователя в социальной сети
        username: { type: String },          // Имя пользователя
        first_name: { type: String },        // Имя
        last_name: { type: String },         // Фамилия
        email: { type: String },             // Адрес электронной почты
        avatar: { type: String },            // Ссылка на аватар
        birthday: { type: Date },            // Дата рождения
        appid: { type: String },             // ID приложения
        access_token: { type: String },      // Токен доступа
        refresh_token: { type: String },     // Токен обновления
        expires_at: { type: Date },          // Время истечения срока действия токена
        scope: { type: String },             // Область доступа
        credentials: { type: String },       // Учетные данные
        last_update: { type: Date },         // Дата последнего обновления
        created_at: { type: Date },          // Дата создания
        is_enrolled: { type: Boolean },      // Зарегистрирован ли пользователь?
        gender: { 
          type: Number, 
          enum: [0, 1, 2]                    // Пол: неизвестно, женский, мужской
        },
        soc_type: { 
          type: String, 
          enum: ['fb', 'ya', 'sb', 'tg', 'tn', 'go', 'tw', 'vk', 'od', 'rn', 'un'] // Тип социальной сети: Facebook, Yandex и т.д.
        }
      }],
      devices: [{
        name: { type: String },              // Имя устройства
        browser: { type: String },           // Браузер, с которого происходит доступ
        ip_address: { type: String },        // IP-адрес устройства
        expires_at: { type: Date },          // Время истечения срока действия устройства
        created_at: { type: Date },          // Дата регистрации устройства
        is_active: { type: Boolean },        // Устройство активно?
        devtype: { 
          type: Number, 
          enum: [0, 1, 2, 3, 4]              // Тип устройства: мобильное, настольное и т.д.
        }
      }]
    }, { timestamps: true });
    ```


## Физическое лицо
Для регистрации физического лица в системе сначала необходимо сохранить всю необходимую информацию в приватное хранилище данных. После сохранения информации в приватное храналище, в блокчейн передаётся идентификатор хранилища и идентификатор данных профиля из этого хранилища.



### Регистрация карточки физлица
!!! note "Регистрация физического лица"
    === "cleos"
        ```bash
        cleos push action registrator reguser '[username, "profile_hash"]' -p username
        ```
    === "js"
        ```javascript
        eos.transact({
            actions: [{
                account: 'registrator',
                name: 'reguser',
                authorization: [{
                    actor: 'username',
                    permission: 'active',
                }],
                data: {
                    username: 'username',
                    data_id: 'profile_hash'
                },
            }]
        }, {
            blocksBehind: 3,
            expireSeconds: 30,
        });
        ```

    **Параметры:** 

    - `username (eosio::name)`: Имя аккаунта пользователя в системе.

    - `profile_hash (std::string)`: Хэш данных зашифрованного профиля в IPFS

### Получение карточки физлица
Для получения карточки физлица, необходимо обратиться по стандартизированному API к хранилищу приватных данных, предоставив цифровую подпись аккаунта, чьи данные запрашиваются, или кооператива, который производил регистрацию. 

!!! note "Получение карточки из хранилища"
    Пример запроса в хранилище

После прохождения криптографической проверки цифровой подписи, хранилище данных вернёт зашифрованную карточку с данными пользователя. Карточка может быть расшифрована только с помощью приватного ключа владельца данных или кооператива, который производил регистрацию. 

!!! note "Пример дешифровки карточки"
    Пример дешифровки


Таким образом, храналища данных хранят зашифрованную информацию в карточках пользователей, но не обладают к нем доступа. 


## Юридическое лицо
Вся информация о юридических лицах хранится в блокчейне в открытом виде. 

### Регистрация карточки юрлица

!!! note "Регистрация юридического лица"
    === "cleos"
        ```bash
        cleos push action registrator regorg '[
          "username",
          "full_name",
          "short_name",
          "legal_address",
          "ogrn_value",
          "inn_value",
          "logo_url",
          "phone_number",
          "email_address",
          "registration_date",
          "website_url",
          [{
            "account_number",
            "created_date",
            "last_update_date",
            "is_active"
          }],
          is_cooperative,
          "optional_coop_type_name",
          "optional_token_contract_name",
          "optional_slug_value",
          "optional_announce_text",
          "optional_description_text",
          optional_members_number,
          "optional_initial_amount",
          "optional_minimum_amount",
          "optional_membership_fee",
          optional_period_value
        ]' -p username
        ```

    === "js"
        ```javascript
        eos.transact({
            actions: [{
                account: 'registrator',
                name: 'regorg',
                authorization: [{
                    actor: 'username',
                    permission: 'active',
                }],
                data: {
                    username: 'username',
                    name: 'full_name',
                    short_name: 'short_name',
                    address: 'legal_address',
                    ogrn: 'ogrn_value',
                    inn: 'inn_value',
                    logo: 'logo_url',
                    phone: 'phone_number',
                    email: 'email_address',
                    registration: 'registration_date',
                    website: 'website_url',
                    accounts: [{
                        account: 'account_number',
                        created_at: 'created_date',
                        last_update: 'last_update_date',
                        is_active: is_active
                    }],
                    is_cooperative: is_cooperative,
                    coop_type: 'optional_coop_type_name',
                    token_contract: 'optional_token_contract_name',
                    slug: 'optional_slug_value',
                    announce: 'optional_announce_text',
                    description: 'optional_description_text',
                    members: optional_members_number,
                    initial: 'optional_initial_amount',
                    minimum: 'optional_minimum_amount',
                    membership: 'optional_membership_fee',
                    period: optional_period_value
                },
            }]
        }, {
            blocksBehind: 3,
            expireSeconds: 30,
        });
        ```

    **Параметры:** 

    - `username (eosio::name)`: Имя аккаунта в системе.

    - `name (std::string)`: Полное наименование.

    - `short_name (std::string)`: Краткое наименование.

    - `address (std::string)`: Юридический адрес.

    - `ogrn (std::string)`: ОГРН.

    - `inn (std::string)`: ИНН.

    - `logo (std::string)`: Ссылка на логотип.

    - `phone (std::string)`: Номер телефона.

    - `email (std::string)`: Email-адрес.

    - `registration (std::string)`: Дата регистрации юрлица.

    - `website (std::string)`: Ссылка на веб-сайт.

    - `accounts (bank)`: Список банковских счетов, который включает в себя:
      - `account`: Номер расчётного счёта.
      - `created_at`: Дата создания.
      - `last_update`: Дата последнего обновления.
      - `is_active`: Активен ли счет.

    - `is_cooperative (bool)`: Является ли кооперативом (true | false).

    - `coop_type (eosio::name)`: Тип кооператива (опционально). `'union' - союз кооперативов | 'conscoop' - потребительский кооператив | 'prodcoop' - производственный кооператив | 'agricoop' - с/х кооператив | 'builderscoop' - строительный кооператив`

    - `token_contract (eosio::name)`: Контракт токена пая (опционально).

    - `slug (std::string)`: Слаг (опционально).

    - `announce (std::string)`: Анонс (опционально).

    - `description (std::string)`: Описание (опционально).

    - `initial`: Вступительный взнос (опционально).

    - `minimum`: Минимальный взнос (опционально).

    - `membership`: Членский взнос (опционально).

    - `period`: Периодичность (опционально).

### Получение карточки юрлица
Для получения карточки юридического лица необходимо выполнить команду:

!!! note "Команда получения карточки юридического лица из блокчейна"
    === "cleos"
        ```bash
        cleos get table registrator registrator orgs
        ```
    === "js"
        ```javascript
        eos.getTableRows({
            code: "registrator",
            scope: "registrator",
            table: "orgs",
            json: true,
        }).then(result => {
            console.log(result);
        }).catch(error => {
            console.error(error);
        });
        ```

    **Ответ:**

    ```json
    {
      "rows": [{
          "username": "имя_аккаунта_кооператива",
          "parent_username": "имя_родительского_пользователя (для кооперативных участков)",
          ...
          "website": "веб-сайт",
          "accounts": [{"account": "номер_счёта", ...}],
        }
      ]
    }
    ```


## Подать заявление на вступление
!!! note ""
    Регистратор - кооператив, производящий регистрацию нового пайщика.


1. Получение карточки заявителя
Первым делом регистратор получает карточку заявителя. Если заявитель является юридическим лицом, регистратор получает данные из блокчейна. Если заявитель – физическое лицо, тогда регистратор запрашивате доступ к зашифрованной карточке с приватными данными из хранилища физического лица в IPFS.

2. Формирование шаблона заявления.
После получения карточки, регистратор создает шаблон заявления с заполненными данными. Шаблон заявления является типовым для всех цифровых кооперативов на платформе.

3. Чтение и подпись.
Заявитель ознакамливается с содержанием предложенного ему документа и оставляет собственноручную подпись на экране мобильного устройства. 

4. Сохранение заявления.
Документ с подписью заявителя сохраняется в формате PDF в зашифрованном виде в приватном хранилище пользователя и передается регистратору с возможностью расшифровки только им по сети IFPS с указанием адреса на скачивание в блокчейне.

5. Активация аккаунта на блокчейне.
Для завершения регистрации пайщика, заявитель подписывает и отправляет транзакцию в блокчейн, указывая ссылку на подписанное и зашифрованное им заявление в IPFS. Это заявление могут расшифровать только заявитель и кооператив, в который вступает пользоваель.

!!! note "Пример транзакции на вступление в кооператив"
    === "cleos"
        ```bash
        cleos push action registrator joincoop '[
          "coop_account_name",
          "applicant_account_name",
          "desired_position_title",
          "position_name",
          "application_draft_data",
          "signature_data"
        ]' -p applicant_account_name
        ```

    === "js"
        ```javascript
        eos.transact({
            actions: [{
                account: 'registrator',
                name: 'joincoop',
                authorization: [{
                    actor: 'applicant_account_name',
                    permission: 'active',
                }],
                data: {
                    coop_username: 'coop_account_name',
                    username: 'applicant_account_name',
                    position_title: 'desired_position_title',
                    position: 'position_name',
                    draft_data: 'application_draft_data',
                    sign: 'signature_data'
                },
            }]
        }, {
            blocksBehind: 3,
            expireSeconds: 30,
        });
        ```

    **Параметры:** 

    - `coop_username (eosio::name)`: Имя аккаунта кооператива.
    - `username (eosio::name)`: Имя аккаунта пайщика.
    - `position_title`: Название желаемой должности.
    - `position (eosio::name)`: Название должности в системе. 'chairman' - председатель, 'vpchairman' - зампредседателя, 'director' - директор, 'vpdirector' - замдиректора, 'boardmember' - член совета, 'execmember' - исполнительный член совета ?, 'votingmember' - голосующий участник, 'assocmember' - ассоциированный участник
    - `ricardian_data`: Публичные переменные подписанного заявления.
    - `statement_hash`: Хэш подписанного заявления.

После отправки транзакции в блокчейн, информация об этом поступает в совет кооператива для рассмотрения. После принятия решения о приёме пайщика, аккаунт получает полный доступ к возможностям экосистемы. 
 -->