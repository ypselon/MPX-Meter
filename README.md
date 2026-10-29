# MPX Meter · Yps Broadcast

**Monitorização e análise de emissões FM com RTL-SDR — Windows x64 · v0.32**

O MPX Meter reúne receção FM, análise do sinal composto MPX, descodificação RDS, escuta estéreo e registo de medições numa aplicação para Windows. Permite acompanhar a modulação, a potência MPX, o piloto estéreo, a banda RDS e os espectros RF/MPX.

A aplicação funciona localmente, sem GNU Radio, Radioconda ou Python. A distribuição é portátil: extrair a pasta e abrir o executável. Requer o runtime .NET indicado abaixo e, para receber emissões reais, um recetor RTL-SDR com controlador USB adequado.

> Versão de monitorização em desenvolvimento. Os cálculos foram ensaiados com sinais sintéticos; a precisão em receção física ainda requer validação. As leituras não substituem um analisador de modulação calibrado.

## Descarregar

Abrir a [Release v0.32](https://github.com/ypselon/MPX-Meter/releases/tag/v0.32) e descarregar **MPX-Meter-0.32-Minimal-Windows-x64.zip**.

O repositório é privado: é necessário iniciar sessão numa conta com acesso autorizado. Os ficheiros automáticos “Source code” do GitHub são arquivos do conteúdo do repositório; para executar a app, escolher o ZIP **Minimal-Windows-x64**.

## Requisitos

| Elemento | Necessário |
| --- | --- |
| Sistema operativo | Windows 10 ou 11, de 64 bits |
| Runtime | .NET 8 **Desktop Runtime**, Windows **x64** |
| Receção de rádio | RTL-SDR USB; modelo de referência: **RTL-SDR Blog V3** |
| Controlador USB | **WinUSB** na Interface 0 da RTL-SDR |
| Antena | Antena adequada à banda FM de 87,5–108 MHz |
| Escuta | Saída de áudio disponível no Windows |

Esta distribuição não é uma versão de 32 bits nem uma compilação para Windows 7. Outros modelos RTL-SDR não têm compatibilidade garantida nesta versão.

**Sem RTL-SDR é possível experimentar a interface em Demonstração**, mas não receber uma estação real. O funcionamento normal não exige ligação à Internet; os downloads iniciais exigem.

## 1. Instalar o .NET Desktop Runtime

1. Aceder à [página oficial do .NET 8](https://dotnet.microsoft.com/en-us/download/dotnet/8.0).
2. Na secção **.NET Desktop Runtime**, escolher **Windows x64**.
3. Instalar o runtime e voltar a abrir a aplicação.

Não é necessário instalar o SDK. O .NET Runtime simples, destinado a aplicações de consola, não substitui o Desktop Runtime exigido pela interface Windows.

## 2. Preparar a RTL-SDR com Zadig

**Zadig é a ferramenta de instalação; WinUSB é o controlador utilizado pela aplicação.** Se a RTL-SDR já funciona com WinUSB noutro programa SDR, este passo pode já estar concluído.

1. Ligar a RTL-SDR a uma porta USB e aguardar que o Windows a detete.
2. Fechar SDR#, SDRangel e outros programas que possam estar a utilizar o recetor.
3. Descarregar o Zadig do [site oficial](https://zadig.akeo.ie/) e executá-lo como administrador.
4. Ativar **Options → List All Devices**.
5. Selecionar **a RTL-SDR, Interface 0**. Pode aparecer como **Bulk-In, Interface (Interface 0)**, RTL2832U ou outra designação do dispositivo.
6. Confirmar cuidadosamente o dispositivo selecionado. Não escolher a Interface 1 nem periféricos como teclado, rato ou placa de som.
7. No campo do controlador a instalar, escolher **WinUSB**.
8. Carregar em **Install Driver** ou **Replace Driver**, conforme o estado atual, e aguardar a conclusão.

O Zadig altera o controlador do dispositivo selecionado: a identificação correta da RTL-SDR é essencial. Uma mudança de porta USB pode exigir nova verificação do controlador.

Consultar também o [guia do fabricante RTL-SDR](https://www.rtl-sdr.com/rtl-sdr-quick-start-guide/). O MPX Meter não instala nem altera controladores automaticamente.

## 3. Extrair e abrir a aplicação

Extrair o ZIP para uma pasta onde o utilizador tenha permissões de escrita. **Não executar diretamente dentro do ZIP.**

Manter estes cinco ficheiros juntos:

| Ficheiro | Função |
| --- | --- |
| `MPX-Meter.exe` | Arranque da aplicação |
| `MpxMeter.Desktop.dll` | Aplicação e processamento de sinal |
| `MpxMeter.Desktop.runtimeconfig.json` | Configuração do runtime .NET |
| `rtlsdr.dll` | Comunicação com a RTL-SDR |
| `LICENSE` | Licenças e avisos das dependências |

Abrir **MPX-Meter.exe**. Não é um executável único: as DLLs e o ficheiro runtimeconfig devem permanecer na mesma pasta. Não há instalador próprio; o runtime e o controlador USB são preparados separadamente.

## 4. Primeira receção

1. Ligar a antena à RTL-SDR.
2. Abrir a roda dentada **Definições**.
3. Em **Fonte**, selecionar **RTL-SDR USB**.
4. Carregar em **Procurar USB** e selecionar o recetor.
5. Configurar ganho/AGC, correção PPM e filtro RF, se necessário.
6. Escolher a saída de áudio e carregar em **Guardar e fechar**.
7. Na sintonia principal, introduzir a frequência da estação.
8. Carregar em **Iniciar**.
9. Aguardar a aquisição dos dados RDS e os **60 segundos** da janela de potência MPX.

RDS e leituras rápidas podem aparecer antes de completar os 60 segundos. A potência longa fica provisória enquanto a janela estiver incompleta.

### Experimentar sem recetor

Em **Definições → Fonte**, escolher **Demonstração**, guardar e carregar em **Iniciar**.

O sinal sintético inclui FM estéreo e RDS de teste: PI **8201**, PS **MPX DEMO** e RadioText de demonstração. Estas indicações não correspondem a uma emissão recebida por antena.

## Sintonia e controlo da receção

- Frequência de **87,50 a 108,00 MHz**, com duas casas decimais.
- **<< / >>:** descer/subir 1 MHz.
- **< / >:** descer/subir 100 kHz.
- O campo central também aceita escrita e confirmação com Enter.
- A sintonia pode mudar durante a receção. As alterações são agrupadas por um breve atraso e a aquisição é reaberta na nova configuração.
- Mudar frequência ou ganho interrompe brevemente o áudio e reinicia as medições, o RDS e a janela de 60 segundos.
- Para mudar fonte, dispositivo, PPM ou filtro, parar primeiro a receção.
- O indicador superior identifica o estado, sem repetir a frequência.

A sintonia RF fica indisponível em Demonstração.

## Painel Recetor RDS

O quadro principal apresenta o nome da estação, RadioText e os dados essenciais de identificação e sinalização.

| Campo | Significado |
| --- | --- |
| **PI** | Código hexadecimal de identificação do programa |
| **PS** | Nome de serviço RDS, até 8 caracteres |
| **RT / RadioText** | Texto emitido pela estação, como programa, artista ou título |
| **PTY / PTYN** | Tipo de programa e respetivo nome, quando transmitido |
| **TP / TA** | Serviço com informação de trânsito / anúncio de trânsito |
| **Música/Fala** | Sinalização M/S enviada pela estação |
| **CT** | Data, hora e informação de desfasamento horário recebidas por RDS |

A vista **RDS** disponibiliza ainda AF, DI, ECC, idioma, EON, identificação de aplicações ODA e dados RT+ suportados, quando presentes na emissão. Inclui estado de sincronismo, contagem de grupos e erros CRC. Os detalhes podem ser copiados por Ctrl+C ou pelo menu de contexto.

A informação depende do que a estação transmite e da qualidade da receção. Campos vazios ou parciais não significam necessariamente avaria. PS e RT podem precisar de vários grupos válidos para completar o texto.

### Medidores de receção

- **Canal · dBFS:** potência média de sinal e ruído após o filtro RF.
- **SNR · dB est.:** relação sinal/ruído estimada. Não é a SNR do áudio; estações adjacentes e ruído não uniforme podem afetar o resultado. Pode ficar sem leitura quando as referências não são válidas ou há clipping significativo.
- **I/Q total · dBFS:** energia global capturada antes da seleção de canal. Pode aumentar devido a outras estações presentes na captura.

Os níveis são relativos à escala digital, sem calibração em dBm. Não existe um valor I/Q universal que garanta boa receção.

## Modulação e potência MPX

MPX é o sinal composto demodulado, antes da desacentuação e da descodificação estéreo. Inclui áudio, piloto e RDS.

| Leitura | O que apresenta |
| --- | --- |
| **MPX 60 s** | Potência média móvel sobre 60 segundos, em dBr |
| **MPX 1 s** | Média de potência de um segundo; atualiza após cada segundo completo |
| **+Mod / −Mod** | Extremos positivo e negativo de cada bloco de 100 ms, em kHz e percentagem |
| **Pico** | Maior valor absoluto das duas polaridades no último segundo |
| **Piloto %** | Nível espectral do piloto de 19 kHz, em percentagem equivalente de ±75 kHz |
| **RDS % eq.** | Energia de 54–60 kHz convertida em percentagem de pico equivalente |
| **RMS** | Desvio eficaz do MPX num bloco de 100 ms |
| **Máx + / −** | Extremos por polaridade desde o início da sessão |
| **RDS kHz RMS** | Valor eficaz integrado na banda de 54–60 kHz |

**100% de modulação corresponde a ±75 kHz.** A referência de potência **0 dBr** corresponde a um seno com desvio de pico ±19 kHz, equivalente a cerca de **13,44 kHz RMS**. As médias de potência são calculadas em unidades lineares.

O RMS e os extremos de 100 ms não são médias de 60 segundos. Uma leitura elevada num bloco curto não determina, por si só, o resultado da janela longa.

O nível RDS equivalente inclui ruído e interferência nessa banda; não é o pico real da modulação RDS nem uma taxa de erros. O piloto e o RDS são estimativas espectrais.

## Gráficos

- **Espectro RF:** visualização da captura I/Q, com escala dBFS por bin e marca do canal sintonizado. Na receção real, o eixo mostra MHz; na demonstração, offsets em kHz.
- **Espectro MPX:** 0–60 kHz, com referências L+R, L−R, piloto a 19 kHz e RDS a 57 kHz.
- **MPX Multiplex:** forma de onda/envelope temporal do composto, em janelas de 50 ms e referências em ±75 kHz. Escolher em **Definições → Gráfico MPX**.
- **Potência MPX:** histórico de 120 segundos, com potência de 100 ms e média móvel de 60 segundos.
- **Áudio L/R:** medidores RMS e pico dos canais esquerdo e direito.

Nos espectros RF e MPX, **Mín.** ajusta o limite inferior entre **−120 e −40 dB**, em passos de 10. Os limites são independentes e guardados. Este ajuste muda apenas a escala visual; não aumenta o ganho nem altera as medições.

Os gráficos podem receber até 20 imagens por segundo; a fluidez efetiva depende do computador e do Windows.

## Ouvir a estação

1. Escolher a placa em **Definições → Saída de áudio**.
2. Iniciar a receção.
3. Ativar **Ouvir** na barra principal e ajustar o volume.
4. Usar **Silenciar** para desligar a escuta sem parar as medições.

A saída pode ser a predefinida do Windows ou uma placa selecionada. Pode ser alterada durante a escuta. O volume não altera a potência MPX nem o volume geral do Windows.

O áudio usa 48 kHz estéreo e desacentuação de 50 µs. O estado estéreo depende do sincronismo do piloto; sem sincronismo, a aplicação apresenta mono nos dois canais.

## Definições, ganho e configuração guardada

A roda dentada reúne fonte, USB, saída de áudio, ganho/AGC, ganho manual, PPM, filtro RF, vista MPX, língua e arranque automático.

- **Ganho:** Manual, Tuner AGC, RTL AGC ou Ambos AGC. O máximo manual do sintonizador fornecido é 49,6 dB; a app usa um valor suportado pelo dispositivo.
- **AGC:** regula automaticamente o ganho; não garante melhor SNR nem acrescenta ganho RF acima do máximo manual.
- **Filtro RF:** 320 ou 280 kHz. A opção mais estreita pode ajudar com canais adjacentes, mas também altera a resposta a modulações largas.
- **PPM:** correção do erro de frequência do recetor.
- **Língua:** Português (Portugal), por defeito, ou English (International). Os textos recebidos da estação mantêm o conteúdo original.
- **Iniciar receção ao abrir:** recupera e inicia a última fonte guardada; vem desligado por defeito.

A configuração é criada em **settings.xml**, junto do executável, e guardada ao fechar as Definições ou a aplicação. Inclui sintonia, ganho, áudio, vista, escalas e preferências. A configuração anterior fica em **settings.xml.bak**.

Se o USB guardado estiver indisponível, a app aguarda nova seleção. Ao atualizar para outra pasta, copiar o settings.xml anterior para conservar as preferências. Não partilhar esse ficheiro se não quiser divulgar a configuração pessoal.

## Registo CSV e diagnóstico

**Gravar CSV…** inicia um registo de medições e RDS, aproximadamente uma linha por segundo. **Fechar CSV** termina a gravação. A demonstração fica identificada no registo. Os campos rápidos registados representam o bloco de medição selecionado, não todos os blocos de 100 ms.

No rodapé:

- **I/Q:** nível digital global.
- **Clipping:** percentagem de pares I/Q próximos dos limites do ADC; 0% é possível e não exclui saturação analógica no sintonizador.
- **DSP:** tempo de processamento relativo à duração do sinal; não é a utilização global da CPU.
- **Janela:** progresso até completar os 60 segundos.

## Resolução de problemas

| Situação | Verificar |
| --- | --- |
| A aplicação não abre ou pede .NET | Instalar .NET 8 Desktop Runtime **x64** e extrair todo o ZIP |
| RTL-SDR não aparece | Ligação USB, seleção do dispositivo, WinUSB na Interface 0 e **Procurar USB** |
| USB ocupado / erro ao iniciar | Fechar outros programas SDR que utilizem o recetor |
| Sinal fraco ou ruidoso | Antena, cabo, frequência e ganho/AGC; ganho máximo nem sempre dá a melhor receção |
| Sem som | Receção iniciada, **Ouvir** ativo, volume, placa selecionada e misturador do Windows |
| RDS incompleto | Aguardar mais grupos, melhorar receção e confirmar que a estação transmite RDS |
| MPX ainda provisório | Aguardar 60 segundos sem reiniciar sintonia/ganho |
| Valor muda face ao SDR# | Comparar ganho, AGCs, filtros, FFT e referências; dBFS/bin não é potência total |
| Preferências não ficam guardadas | Usar uma pasta com permissões de escrita |

## Âmbito e licenças

A app destina-se à monitorização FM. Ruído, multipercurso, canais adjacentes, saturação e filtros podem afetar os resultados. O zoom dos gráficos não calibra o recetor e uma janela de 60 segundos completa não certifica conformidade da emissão.

A identificação **Yps Broadcast** não constitui assinatura digital Authenticode. O pacote mínimo inclui os avisos e licenças no ficheiro **LICENSE**. Este repositório distribui o executável e documentação; o código-fonte da aplicação não está publicado aqui.
