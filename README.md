# [ 🚩」 Extreme Hacking CTF 2023: Desafio Ap0t30s3

(images/logo extreme.png)

INTRODUÇÃO
O Capture the Flag (CTF) do Extreme Hacking 2023, Desafio Ap0t30s3, foi um emocionante evento de segurança cibernética onde os participantes enfrentaram diversos desafios em categorias como web, criptografia, lógica, esteganografia entre outros. Os jogadores do time HackMages, conhecidos como 4l174 e WLSF compartilharam suas estratégias e soluções dos desafios sobre engenharia reversa. O CTF destacou-se pela sua diversidade e complexidade, proporcionando aos competidores a oportunidade de aprender, crescer e se divertir enquanto exploravam o mundo da segurança cibernética.

Eng. Reversa
Try_Harder (400)

Descrição
Dentro deste arquivo executável para Unix, há uma flag escondida. Nossa missão é decifrar o código e encontrar essa flag secreta.

Solução detalhada
O arquivo chegou sem uma extensão clara. Para determinar se era um executável destinado a sistemas Linux ou Windows, utilizamos o comando "file TryHarder" no ambiente Kali Linux, o qual forneceu informações detalhadas sobre o arquivo.



Ao constatar que o arquivo em questão estava no formato ELF (Executable and Linkable Format), um formato de arquivo usado em sistemas operacionais Unix e Unix-like para armazenar executáveis, bibliotecas compartilhadas e objetos relocáveis, tomamos as medidas necessárias para sua análise. Para esse fim, configuramos uma máquina virtual Ubuntu com o depurador (gdb) para analisar e depurar o arquivo.




Empregamos o comando "strings" para exibir todas as sequências de caracteres presentes no arquivo.



Utilizei o comando > string.txt para redirecionar a saída das strings para um arquivo que denominei como "strings", a fim de melhorar a legibilidade.






Ao executarmos o comando "ls -l" no arquivo em questão, verificamos suas permissões e observamos que não possuía permissão de execução. Portanto, procedemos a conceder permissões de execução utilizando o comando "chmod 777".





Após conseguir executar o arquivo, iremos depurar utilizando o GDB no sistema Ubuntu. O GDB não é pré-instalado por padrão, portanto, empregamos os seguintes comandos para realizar a instalação:

sudo apt-get update
sudo apt-get install gdb

Desta forma, garantimos que o GDB esteja disponível para depurar o programa conforme necessário.






Após a conclusão da instalação, iniciamos o processo de depuração do arquivo utilizando o comando "gdb TryHarder".




O código não estava muito legível, por isso utilizamos o comando "set disassembly-flavor intel" para melhorar a sua compreensão. O comando "set disassembly-flavor intel" configura o depurador para exibir o código de montagem no formato Intel, tornando-o mais legível para programadores familiarizados com essa notação.



Após examinar o binário do arquivo, identificamos que ele realiza a abertura de um arquivo específico. Para entender melhor o conteúdo deste arquivo, começamos por localizar o primeiro endereço de memória virtual associado a uma das variáveis. Em seguida, acessamos o valor contido nesse endereço e solicitamos uma representação em forma de string, utilizando o comando "x /s 0x48d008", seguido da execução com o comando "r".

Já no primeiro endereço, obtivemos um nome de arquivo. Ao analisar o código-fonte, constatamos que este nome de arquivo é crucial para a execução do arquivo "TryHarder". Isso evidencia que a mensagem "Não foi possível abrir o arquivo." é simplesmente uma string gerada por uma estrutura condicional.



Criei o arquivo no mesmo diretório onde o arquivo principal está localizado utilizando o comando "touch".







Executamos o comando "disassemble main" para visualizar o código de montagem da função principal. O comando "disassemble main" exibe o código de montagem da função principal de um programa, permitindo a análise de como o código de alto nível é traduzido em instruções de baixo nível em assembly. Isso é útil para a depuração e compreensão detalhada da execução do programa.




O arquivo foi executado e produziu uma "string criptografada". Retornamos ao código assembly e localizamos o método chamado SHA256. Em seguida, realizamos um desmontagem (disassemble) do método SHA256 para entender o que estava ocorrendo.

Para determinar o texto que o código estava criptografando, examinamos as variáveis nas linhas 64, 67 e 70. Adicionamos um ponto de interrupção (breakpoint) na linha 73 usando o comando "SHA256:73" e executamos o programa com "r", o qual parou no nosso ponto de interrupção. Em seguida, usamos "i registers" para inspecionar os valores dos registradores e obtivemos o endereço de memória, localizando a string original usando "x /s" e o número da coluna do meio. Realizamos esse processo para as variáveis rdx, rsi e rdi. 





Na variável rdi, identificamos um padrão de saída semelhante a Base64 e, em seguida, a decodificamos online, o que nos forneceu algo semelhante a uma flag, mas ainda não estava completamente decifrada.



Após um esforço significativo, surgiram suspeitas de que a mensagem poderia estar cifrada usando uma cifra de César, devido à presença de uma sequência de caracteres não aleatórios e repetitivos. Para confirmar nossa suspeita, recorremos a um decifrador online por força bruta, o que resultou na confirmação da cifra. Finalmente, conseguimos decifrar a mensagem.




N0t_R3lev4nt (100)
Descrição
Dentro deste arquivo de script PowerShell há uma flag escondida. Nossa missão é decifrar o código e encontrar essa flag secreta.

Solução detalhada
Iniciamos examinando o conteúdo do arquivo usando o comando "cat" e prontamente identificamos o que parecia ser uma sequência de caracteres codificada em Base64 invertido. Aplicamos a inversão da string usando uma ferramenta online e, em seguida, a decodificamos usando um decodificador Base64 online para verificar nossa suspeita, o que resultou em sucesso. Ao examinar o código decodificado, encontramos uma variável suspeita denominada "$maybe_flag" e decidimos testá-la para confirmar se essa era de fato a flag, o que se revelou correto.











