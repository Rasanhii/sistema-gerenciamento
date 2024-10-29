# Sistema de Gerenciamento de Biblioteca 📚

![biblioteca](https://github.com/user-attachments/assets/2aafe9bc-0b67-4db4-9f1b-64c01cb3bf4a)



**1. Estrutura da Aplicação com Padrão MVC**

No padrão MVC (Model-View-Controller):

- Model (Modelo): Define as classes de dados (Livro e Empréstimo) e mantém a lógica de negócios.
- View (Visão): Fornece a interface com o usuário, onde se insere informações e visualiza os resultados (no caso, simulada através de uma aplicação de linha de comando).
- Controller (Controlador): Manipula a entrada do usuário e chama os métodos apropriados no modelo.

**2. Definição das Classes do Modelo, Visão e Controlador**

Aqui está a definição das principais classes:

Modelo
```Javascript
class Livro:
    def __init__(self, titulo, autor, isbn):
        self.titulo = titulo
        self.autor = autor
        self.isbn = isbn
        self.emprestado = False

class Emprestimo:
    def __init__(self, isbn, usuario):
        self.isbn = isbn
        self.usuario = usuario
```

Controlador
```javascript
class BibliotecaController:
    def __init__(self):
        self.livros = []
        self.emprestimos = []

    def cadastrar_livro(self, titulo, autor, isbn):
        livro = Livro(titulo, autor, isbn)
        self.livros.append(livro)

    def consultar_livros(self, termo_busca):
        return [livro for livro in self.livros 
                if termo_busca.lower() in livro.titulo.lower() or termo_busca.lower() in livro.autor.lower()]

    def realizar_emprestimo(self, isbn, usuario):
        for livro in self.livros:
            if livro.isbn == isbn and not livro.emprestado:
                livro.emprestado = usuario
                self.emprestimos.append(Emprestimo(isbn, usuario))
                return f"Livro '{livro.titulo}' emprestado para {usuario}"
        return "Livro indisponível ou não encontrado."

    def realizar_devolucao(self, isbn):
        for livro in self.livros:
            if livro.isbn == isbn and livro.emprestado:
                livro.emprestado = False
                self.emprestimos = [emp for emp in self.emprestimos if emp.isbn != isbn]
                return f"Livro '{livro.titulo}' devolvido com sucesso."
        return "Livro não encontrado ou não está emprestado."
```

**3. Fluxo de Eventos da Aplicação**

**Cadastro de Livro**

1) O usuário informa título, autor e ISBN.
2) A visão captura os dados e envia para o controlador.
3) O controlador chama cadastrar_livro() do modelo, que cria o objeto Livro e o armazena.

**Consulta de Livro**

1) O usuário insere um termo de busca (título ou autor).
2) A visão captura o termo e chama o controlador.
3) O controlador executa consultar_livros() e retorna a lista de livros que correspondem ao termo de busca.

**Empréstimo de Livro**

1) O usuário informa o ISBN e o nome.
2) A visão captura os dados e envia para o controlador.
3) O controlador executa realizar_emprestimo(), atualizando o status do livro e registrando o empréstimo.

**Devolução de Livro**

1) O usuário informa o ISBN.
2) A visão captura o ISBN e envia para o controlador.
3) O controlador executa realizar_devolucao(), alterando o status do livro e removendo o empréstimo.

**5. Diagrama de Sequência UML (PlantText)**

![diagrama]("public/diagrama2.png")

