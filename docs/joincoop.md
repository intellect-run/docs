---
title: Вступить в пайщики
---
После того, как вы получили статус верифицированного кооператива, вы можете регистрировать физические и юридические лица в свой кооператив. 


## Хранилища данных
Описание принципов работы и список доступных хранилищ

### Модель приватных данных пользователя
Модель данных для хранения в приватном хранилище должна следовать стандартизированной структуре. На основе этой структуры данных в дальшейшем любой клиент сможет восстановить ваш электронный документ при наличии соответствующих прав доступа, определяющихся приватными ключами доступа к аккаунту.


!!! note "Стандарт модели"
    ```javascript
    const userDataSchema = new Schema({
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
        cleos push action registrator reguser '[username, storage_id, "data_id"]' -p username
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
                    storage_id: storage_id_value,
                    data_id: 'data_id_value'
                },
            }]
        }, {
            blocksBehind: 3,
            expireSeconds: 30,
        });
        ```

    **Параметры:** 

    - `username (eosio::name)`: Имя аккаунта пользователя в системе.

    - `storage_id (uint64_t)`: Идентификатор приватного хранилища данных.

    - `data_id (std::string)`: Идентификатор данных в приватном хранилище.



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


## Подать заявление на вступление



