# Basic

# Organize-all-files-into-subfolders-.
This script will analyze a folder you specify and organize all files into subfolders categorized by extension. It also handles important details like creating folders, handling files without extensions, and preventing conflicts with duplicate file names.

Script

import os
import shutil

def organizar_arquivos(caminho_pasta):
    """
    Organiza os arquivos em uma pasta específica, movendo-os para subpastas
    baseadas em categorias de extensão.

    Args:
        caminho_pasta (str): O caminho absoluto para a pasta a ser organizada.
    """
    # Validação inicial para garantir que o caminho fornecido é uma pasta válida.
    if not os.path.isdir(caminho_pasta):
        print(f"Erro: O caminho '{caminho_pasta}' não é uma pasta válida ou não existe.")
        return

    # Dicionário que mapeia as categorias de arquivos às suas respectivas extensões.
    # Isso torna o código mais limpo e fácil de manter.
    CATEGORIAS = {
        "documentos": [".pdf", ".doc", ".docx", ".odt"],
        "imagens": [".jpg", ".jpeg", ".png", ".gif", ".bmp", ".svg"],
        "vídeos": [".mp4", ".avi", ".mov", ".mpg", ".flv", ".mkv"],
        "áudios": [".mp3", ".wav", ".ogg", ".aac", ".flac"],
        "executáveis": [".exe", ".msi"],
        "compactados": [".zip", ".rar", ".7z", ".tar", ".gz"],
        "planilhas": [".xls", ".xlsx", ".ods"],
        "apresentações": [".ppt", ".pptx", ".odp"],
        "textos": [".txt", ".csv", ".rtf"]
    }

    # Mapeamento reverso de extensão para categoria para otimizar a busca.
    # Em vez de iterar sobre as categorias, podemos buscar a categoria de uma extensão diretamente.
    extensao_para_categoria = {ext: cat for cat, exts in CATEGORIAS.items() for ext in exts}

    # Itera sobre cada item (arquivo ou subpasta) na pasta principal.
    for nome_arquivo in os.listdir(caminho_pasta):
        caminho_arquivo_origem = os.path.join(caminho_pasta, nome_arquivo)

        # Ignora subpastas e processa apenas arquivos.
        if os.path.isfile(caminho_arquivo_origem):
            # Extrai a extensão do arquivo em minúsculas para consistência.
            _, extensao = os.path.splitext(nome_arquivo)
            extensao = extensao.lower()

            # Determina a categoria do arquivo. Se a extensão não for encontrada,
            # a categoria padrão é 'outros'.
            categoria = extensao_para_categoria.get(extensao, "outros")

            # Cria o caminho completo para a subpasta de destino.
            caminho_pasta_destino = os.path.join(caminho_pasta, categoria)

            # Verifica se a subpasta de destino existe e, se não, a cria.
            # O `exist_ok=True` previne erros caso a pasta já exista.
            os.makedirs(caminho_pasta_destino, exist_ok=True)

            # Monta o caminho final para onde o arquivo será movido.
            caminho_arquivo_destino = os.path.join(caminho_pasta_destino, nome_arquivo)

            # Lógica para lidar com nomes de arquivos duplicados.
            # Se um arquivo com o mesmo nome já existir no destino, um sufixo numérico é adicionado.
            contador = 1
            nome_base, extensao_original = os.path.splitext(nome_arquivo)
            while os.path.exists(caminho_arquivo_destino):
                # Cria um novo nome de arquivo com um sufixo incremental.
                novo_nome_arquivo = f"{nome_base}_{contador}{extensao_original}"
                caminho_arquivo_destino = os.path.join(caminho_pasta_destino, novo_nome_arquivo)
                contador += 1
            
            # Move o arquivo para a pasta de destino correta.
            shutil.move(caminho_arquivo_origem, caminho_arquivo_destino)
            print(f"Movido: '{nome_arquivo}' para a pasta '{categoria}'")

    print("\nOrganização de arquivos concluída com sucesso!")

# --- Exemplo de como usar o script ---
if __name__ == "__main__":
    # IMPORTANTE: Substitua o caminho abaixo pelo caminho da pasta que você deseja organizar.
    # Use um caminho absoluto para evitar problemas.
    # Exemplo para Windows: "C:\Users\SeuUsuario\Downloads"
    # Exemplo para macOS/Linux: "/Users/SeuUsuario/Downloads"
    pasta_a_ser_organizada = "C:\caminho\para\sua\pasta" # <-- SUBSTITUA AQUI

    organizar_arquivos(pasta_a_ser_organizada)
