# How to run locally
1. First, install the required system dependencies:
```bash
sudo apt install build-essential python3-dev libffi-dev \
    python3-pip python3-setuptools sqlite3 \
    libssl-dev virtualenv libjpeg-dev libxslt1-dev libicu-dev \
    pkg-config
```

2. Install Rust using rustup (required for building native modules):
```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

3. Install Poetry (the dependency management tool):
```bash
pip install --user pipx
pipx install poetry
```

4. Clone the repository and navigate into it:
```bash
git clone https://github.com/element-hq/synapse.git
cd synapse
```

5. Install the project dependencies using Poetry:
```bash
poetry install --extras all
```

6. Create a configuration file:
```bash
cp docs/sample_config.yaml homeserver.yaml
```

7. Generate a config file for your server:
```bash
poetry run python -m synapse.app.homeserver \
    --server-name my.domain.name \
    --config-path homeserver.yaml \
    --generate-config \
    --report-stats=no
```

### Postgres Setup
In terminal enter postgres of WSL: `psql -U postgres`
```
CREATE USER synapse_user WITH PASSWORD '1q2w3e';
CREATE DATABASE synapse ENCODING 'UTF8' LC_COLLATE='C' LC_CTYPE='C' template=template0 OWNER synapse_user;
```

### Homeserver: 
```
server_name: "afm"
listeners:
  - port: 8888
    tls: false
    type: http
    x_forwarded: true
    bind_addresses: ['::1', '127.0.0.1']
    resources:
      - names: [client, federation]
        compress: false
database:
  name: psycopg2
  args:
    user: synapse_user
    password: 1q2w3e
    database: synapse
    host: localhost
    cp_min: 5
    cp_max: 10

enable_registration: True
enable_registration_without_verification: True
```

8. Open code in WSL: Ubuntu
First enter `wsl` and then type `code .` to open the project in __Code Editor__
9. Start Synapse:
```bash
poetry run python start_synapse.py
```

