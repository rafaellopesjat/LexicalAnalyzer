import re

tokens_types = [
    ("Number", r'\d+(\.\d+)?'),
    ("Boolean", r'\b(true|false)\b'),
    ("Char", r"'[^'\n]'"),
    ("String", r'"[^"\n]*"'),
    ("Identifier", r'[a-zA-Z_][\w]*'),
    ("Operator", r'==|!=|>=|<=|>|<|=|\+|\-|\*|/'),
    ("Separator", r'[()\{\};]')
]

keywords = ["int", "float", "char", "bool", "if", "else", "for", "while", "do", "void"]

def read_file(file_name):
    with open(f'files/{file_name}', 'r', encoding='utf-8') as file:
        return file.read()
    
def extract_lexemas(code):
    code = re.sub(r'#\s*(include\s*<[^>]+>|define\s+\w+.*)', '', code)
    code = re.sub(r'//.*', '', code)
    code = re.sub(r'/\*.*?\*/', '', code, flags=re.DOTALL)

    pattern = r'\d+(?:\.\d+)?|"[^"\n]*"|[a-zA-Z_]\w*|==|!=|>=|<=|>|<|=|\+|\-|\*|/|[()\{\};]|[^\s()\{\};=+\-*/<>!]+'
    lexemas = re.findall(pattern, code)
    return lexemas
    
def add_in_symbol_table(symbol_table, lexema):
    symbol_id = len(symbol_table) + 1
    symbol_table[symbol_id] = lexema

def add_in_tokens_list(tokens, lexema, token_type, is_identifier = False, lexema_id = 1):
    if is_identifier:
        tokens.append((f"id, {lexema_id}", token_type))
    else:
        tokens.append((lexema, token_type))

def add_identifier_in_tokens_list(tokens, lexema, symbol_table, token_type):
    if lexema not in symbol_table.values():
        add_in_symbol_table(symbol_table, lexema)
                    
    lexema_id = next((k for k, v in symbol_table.items() if v == lexema), None)
    add_in_tokens_list(tokens, lexema, token_type, True, lexema_id)
     
def analyzer(code , tokens, symbol_table, errors):
    lexemas = extract_lexemas(code)

    for lexema in lexemas:
        if lexema in keywords:
            add_in_tokens_list(tokens, lexema, "Keyword")
            continue
        
        matched = False

        for token_type, pattern in tokens_types:
            if(re.match(pattern, lexema)):
                matched = True

                if token_type == "Identifier":
                    add_identifier_in_tokens_list(tokens, lexema, symbol_table, token_type)
                    break
                
                add_in_tokens_list(tokens, lexema, token_type)
                break

        if not matched:
            errors.append(f"Erro léxico: '{lexema}' não reconhecido.")

def start_menu():
    print("--------- Analisador Léxico --------- \n")
    return input("Digite o nome do arquivo c que desejar analisar (é necessário que o arquivo esteja na pasta files): ")

def show_results(tokens, symbol_table, errors):
    print("\n--------- Lista de Tokens ---------\n")
    for token in tokens:
        print(token)
    print("\n--------------------------------------\n")

    print("--------- Tabela de Símbolos ---------\n")
    for symbol_id, symbol in symbol_table.items():
        print(f"{symbol_id}: {symbol}")
    print("\n--------------------------------------\n")

    print("---------------- Erros ----------------\n")
    for error in errors:
        print(error)
    print("\n--------------------------------------\n")


file_name = start_menu()

code = read_file(file_name)
tokens = []
symbol_table = {}
errors = []

analyzer(code, tokens, symbol_table, errors)
show_results(tokens, symbol_table, errors)
