# CONTROLE-SERVICO-E-PECAS
import sqlite3

def criar_conexao():
    conn = sqlite3.connect('app.db')
    return conn

def criar_tabelas():
    conn = criar_conexao()
    cursor = conn.cursor()

    cursor.execute('''
    CREATE TABLE IF NOT EXISTS clientes (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        nome TEXT NOT NULL,
        telefone TEXT,
        email TEXT
    )
    ''')

    cursor.execute('''
    CREATE TABLE IF NOT EXISTS pecas (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        nome TEXT NOT NULL,
        codigo TEXT,
        preco REAL NOT NULL,
        estoque INTEGER NOT NULL
    )
    ''')

    cursor.execute('''
    CREATE TABLE IF NOT EXISTS servicos (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        cliente_id INTEGER,
        descricao TEXT,
        data TEXT,
        valor_mao_obra REAL,
        FOREIGN KEY(cliente_id) REFERENCES clientes(id)
    )
    ''')

    cursor.execute('''
    CREATE TABLE IF NOT EXISTS servico_pecas (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        servico_id INTEGER,
        peca_id INTEGER,
        quantidade INTEGER,
        FOREIGN KEY(servico_id) REFERENCES servicos(id),
        FOREIGN KEY(peca_id) REFERENCES pecas(id)
    )
    ''')

    conn.commit()
    conn.close()

if __name__ == "__main__":
    criar_tabelas()
    print("Tabelas criadas com sucesso!")
