# GerenciamentoBiblioteca
   public string Titulo { get; set; }
    public string Autor { get; set; }
    public string Genero { get; set; }
    public int QuantidadeDisponivel { get; set; }

    public Livro(string titulo, string autor, string genero, int quantidade)
    {
        Titulo = titulo;
        Autor = autor;
        Genero = genero;
        QuantidadeDisponivel = quantidade;
    }


class Usuario
{
    public string Nome { get; set; }
    public int LivrosEmprestados { get; set; } = 0;

    public Usuario(string nome)
    {
        Nome = nome;
    }
}

class Biblioteca
{
    private List<Livro> livros = new List<Livro>();
    private Dictionary<string, Usuario> usuarios = new Dictionary<string, Usuario>();

    public void CadastrarLivro()
    {
        Console.Write("Digite o título do livro: ");
        string titulo = Console.ReadLine();
        Console.Write("Digite o autor do livro: ");
        string autor = Console.ReadLine();
        Console.Write("Digite o gênero do livro: ");
        string genero = Console.ReadLine();
        Console.Write("Digite a quantidade disponível: ");
        int quantidade = int.Parse(Console.ReadLine());

        livros.Add(new Livro(titulo, autor, genero, quantidade));
        Console.WriteLine("Livro cadastrado com sucesso!");
    }

    public void ConsultarCatalogo()
    {
        Console.WriteLine("\n--- Catálogo de Livros ---");
        foreach (var livro in livros)
        {
            Console.WriteLine($"Título: {livro.Titulo}, Autor: {livro.Autor}, Gênero: {livro.Genero}, Disponível: {livro.QuantidadeDisponivel}");
        }
    }

    public void EmprestarLivro(string nomeUsuario)
    {
        if (!usuarios.ContainsKey(nomeUsuario))
        {
            usuarios[nomeUsuario] = new Usuario(nomeUsuario);
        }

        var usuario = usuarios[nomeUsuario];

        if (usuario.LivrosEmprestados < 3)
        {
            Console.Write("Digite o título do livro que deseja emprestar: ");
            string titulo = Console.ReadLine();
            var livro = livros.Find(l => l.Titulo.Equals(titulo, StringComparison.OrdinalIgnoreCase));

            if (livro != null && livro.QuantidadeDisponivel > 0)
            {
                livro.QuantidadeDisponivel--;
                usuario.LivrosEmprestados++;
                Console.WriteLine($"Livro '{titulo}' emprestado com sucesso!");
            }
            else
            {
                Console.WriteLine("Livro não disponível ou não encontrado.");
            }
        }
        else
        {
            Console.WriteLine("Limite de 3 livros emprestados atingido.");
        }
    }

    public void DevolverLivro(string nomeUsuario)
    {
        if (usuarios.ContainsKey(nomeUsuario))
        {
            Console.Write("Digite o título do livro que deseja devolver: ");
            string titulo = Console.ReadLine();
            var livro = livros.Find(l => l.Titulo.Equals(titulo, StringComparison.OrdinalIgnoreCase));

            if (livro != null)
            {
                livro.QuantidadeDisponivel++;
                usuarios[nomeUsuario].LivrosEmprestados--;
                Console.WriteLine($"Livro '{titulo}' devolvido com sucesso!");
            }
            else
            {
                Console.WriteLine("Livro não encontrado.");
            }
        }
        else
        {
            Console.WriteLine("Usuário não encontrado.");
        }
    }
}

class Program
{
    static void Main()
    {
        Biblioteca biblioteca = new Biblioteca();
        string opcao;

        do
        {
            Console.WriteLine("\n--- Sistema de Gerenciamento de Biblioteca ---");
            Console.WriteLine("1. Cadastrar Livro (Administrador)");
            Console.WriteLine("2. Consultar Catálogo");
            Console.WriteLine("3. Emprestar Livro");
            Console.WriteLine("4. Devolver Livro");
            Console.WriteLine("5. Sair");
            Console.Write("Escolha uma opção: ");
            opcao = Console.ReadLine();

            switch (opcao)
            {
                case "1":
                    biblioteca.CadastrarLivro();
                    break;

                case "2":
                    biblioteca.ConsultarCatalogo();
                    break;

                case "3":
                    Console.Write("Digite seu nome: ");
                    string nomeUsuarioEmprestimo = Console.ReadLine();
                    biblioteca.EmprestarLivro(nomeUsuarioEmprestimo);
                    break;

                case "4":
                    Console.Write("Digite seu nome: ");
                    string nomeUsuarioDevolucao = Console.ReadLine();
                    biblioteca.DevolverLivro(nomeUsuarioDevolucao);
                    break;

                case "5":
                    Console.WriteLine("Saindo do sistema.");
                    break;

                default:
                    Console.WriteLine("Opção inválida. Tente novamente.");
                    break;
            }
        } while (opcao != "5");
    }
}

