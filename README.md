#### Шаг 1. Забрать проект к себе

```
git clone https://github.com/ingvarch/ansible-aws-ec2.git
```

#### Шаг 2. Не забыть сделать все необходимые экспорты

```
export AWS_ACCESS_KEY=""
export AWS_SECRET_KEY=""

export AWS_ACCESS_KEY_ID=""
export AWS_SECRET_ACCESS_KEY=""

export AWS_REGION="us-east-1"

export ANSIBLE_HOSTS=./inventory/ec2.py
export EC2_INI_PATH=./inventory/ec2.ini
```

#### Шаг 3. Установить зависимости

```
pip install -r requirements.txt
```

#### Шаг 4. Исправить локальный конфиг если нужно

```
vi ansible-aws-ec2/ansible.cfg
```

#### Шаг 5. Запуск создания vpc, sg, машинок

```
ansible-playbook -i inventory provision_instance.yaml
```

После того, как машинки развернуться, доступ к ним будет доступен только
через сервер который tag_Name_Monitor.

Необходимо на него установить git, pip, скопировать созданный ключ
ansible-aws-ec2/keys и повторить Шаги 1-4
(todo: pre_setup_instance.yaml с этими действиями)

#### Шаг 6.1. Перечитать кэш для ec2.py

```
ansible-aws-ec2/inventory/ec2.py --refresh-cache
```

#### Шаг 6.2. Запуск установки софта

```
ansible-playbook -i inventory setup_instance.yaml
```

#### Шаг 7. Запуск установки системы мониторинга и ее агентов

```
ansible-playbook -i inventory monitor_instance.yaml
```

#### Дальнейшие шаги

1. http://server-ip/zabbix - логин пароль дефолтные Admin/zabbix
2. Руками добавить хосты (todo: автоматически добавлять)

#### TODO:

1. в tasks/instance.yaml есть строчка vpc_subnet_id: "{{ vpc_data.subnets[1].id }}" разобраться с тем как передавать нужный vpc_id.subnet так как каждый раз он подтягивает разный, а их два
2. Скрипты для pre_setup_instance.yaml и добавление заббикс агентов на сервер
3. Динамические private_ip
