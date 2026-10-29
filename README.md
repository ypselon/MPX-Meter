# MPX Meter · Yps Broadcast

**Monitorização e análise de emissões FM com RTL-SDR · v0.32**

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

---

# English — MPX Meter · Yps Broadcast

**FM broadcast monitoring and analysis with RTL-SDR · v0.32**

MPX Meter combines FM reception, composite MPX signal analysis, RDS decoding, stereo listening and measurement logging in a Windows application. It displays modulation, MPX power, the stereo pilot, the RDS band and RF/MPX spectra.

The application runs locally without GNU Radio, Radioconda or Python. It is distributed as a portable folder: extract it and launch the executable. The .NET runtime described below is required. Receiving actual broadcasts also requires an RTL-SDR receiver with the appropriate USB driver.

> This monitoring application is under development. Calculations have been tested with synthetic signals; accuracy during hardware reception still requires validation. Readings do not replace a calibrated modulation analyser.

## Download

Open [Release v0.32](https://github.com/ypselon/MPX-Meter/releases/tag/v0.32) and download **MPX-Meter-0.32-Minimal-Windows-x64.zip**.

This repository is private: sign in with an account authorised to access it. GitHub's automatic “Source code” files are archives of the repository contents; to run the application, choose the **Minimal-Windows-x64** ZIP.

## Requirements

| Item | Requirement |
| --- | --- |
| Operating system | 64-bit Windows 10 or 11 |
| Runtime | .NET 8 **Desktop Runtime**, Windows **x64** |
| Radio reception | RTL-SDR USB; reference model: **RTL-SDR Blog V3** |
| USB driver | **WinUSB** on the RTL-SDR's Interface 0 |
| Antenna | An antenna suitable for the 87.5–108 MHz FM band |
| Listening | An available Windows audio output |

This distribution is neither a 32-bit version nor a Windows 7 build. Compatibility with other RTL-SDR models is not guaranteed in this version.

**Without an RTL-SDR, you can explore the interface in Demo mode**, but cannot receive an actual station. Normal operation does not require an Internet connection; initial downloads do.

## 1. Install the .NET Desktop Runtime

1. Visit the [official .NET 8 download page](https://dotnet.microsoft.com/en-us/download/dotnet/8.0).
2. Under **.NET Desktop Runtime**, select **Windows x64**.
3. Install the runtime and reopen the application.

The SDK is not required. The standard .NET Runtime intended for console applications does not replace the Desktop Runtime required by the Windows interface.

## 2. Prepare the RTL-SDR using Zadig

**Zadig is the installation tool; WinUSB is the driver used by the application.** If the RTL-SDR already works with WinUSB in another SDR program, this step may already be complete.

1. Connect the RTL-SDR to a USB port and wait for Windows to detect it.
2. Close SDR#, SDRangel and any other application that may be using the receiver.
3. Download Zadig from its [official website](https://zadig.akeo.ie/) and run it as administrator.
4. Enable **Options → List All Devices**.
5. Select **the RTL-SDR, Interface 0**. It may appear as **Bulk-In, Interface (Interface 0)**, RTL2832U or another device name.
6. Carefully verify the selected device. Do not select Interface 1 or peripherals such as your keyboard, mouse or sound card.
7. Select **WinUSB** as the driver to install.
8. Click **Install Driver** or **Replace Driver**, depending on the current configuration, and wait for completion.

Zadig changes the driver of the selected device: correctly identifying the RTL-SDR is essential. Moving the receiver to another USB port may require checking the driver again.

See also the [RTL-SDR manufacturer's guide](https://www.rtl-sdr.com/rtl-sdr-quick-start-guide/). MPX Meter does not install or change drivers automatically.

## 3. Extract and launch the application

Extract the ZIP to a folder where your user account has write permission. **Do not run the application from inside the ZIP.**

Keep these five files together:

| File | Purpose |
| --- | --- |
| `MPX-Meter.exe` | Application launcher |
| `MpxMeter.Desktop.dll` | Application and signal processing |
| `MpxMeter.Desktop.runtimeconfig.json` | .NET runtime configuration |
| `rtlsdr.dll` | RTL-SDR communication |
| `LICENSE` | Dependency licences and notices |

Open **MPX-Meter.exe**. This is not a single-file executable: the DLLs and runtimeconfig file must remain in the same folder. There is no application installer; the runtime and USB driver are installed separately.

## 4. First reception

1. Connect the antenna to the RTL-SDR.
2. Open **Settings** using the gear icon.
3. Set **Source** to **RTL-SDR USB**.
4. Click **Find USB** and select the receiver.
5. Configure gain/AGC, PPM correction and the RF filter if needed.
6. Select the audio output and click **Save and close**.
7. Enter the station frequency in the main tuning field.
8. Click **Start**.
9. Allow time for RDS acquisition and for the **60-second** MPX power window to fill.

RDS and short-term readings may appear before the 60 seconds have elapsed. The long-term power reading remains provisional until the window is complete.

### Try it without a receiver

Under **Settings → Source**, select **Demo**, save and click **Start**.

The synthetic signal includes stereo FM and test RDS: PI **8201**, PS **MPX DEMO** and demonstration RadioText. These readings do not represent an over-the-air broadcast.

## Tuning and reception controls

- Frequency range: **87.50 to 108.00 MHz**, displayed with two decimal places.
- **<< / >>:** tune down/up by 1 MHz.
- **< / >:** tune down/up by 100 kHz.
- The central field also accepts typed values, confirmed with Enter.
- Tuning can change during reception. Changes are combined over a short delay before acquisition reopens with the new configuration.
- Changing frequency or gain briefly interrupts audio and resets measurements, RDS and the 60-second window.
- Stop reception before changing the source, device, PPM correction or filter.
- The header indicator shows reception status without repeating the frequency.

RF tuning is unavailable in Demo mode.

## Receiver RDS panel

The main panel shows the station name, RadioText and essential identification and signalling data.

| Field | Meaning |
| --- | --- |
| **PI** | Programme Identification code in hexadecimal |
| **PS** | RDS Programme Service name, up to 8 characters |
| **RT / RadioText** | Text transmitted by the station, such as programme, artist or title |
| **PTY / PTYN** | Programme type and programme type name, when transmitted |
| **TP / TA** | Traffic programme service / traffic announcement |
| **Music/Speech** | M/S signalling transmitted by the station |
| **CT** | Date, time and local time offset information received through RDS |

The **RDS** view also provides AF, DI, ECC, language, EON, ODA application identification and supported RT+ data when present in the broadcast. It includes synchronisation status, group counts and CRC errors. Details can be copied with Ctrl+C or the context menu.

Available information depends on the station's transmission and reception quality. Empty or partial fields do not necessarily indicate a fault. PS and RT may require several valid groups to complete the text.

### Reception meters

- **Channel · dBFS:** average signal-plus-noise power after the RF filter.
- **SNR · dB est.:** estimated signal-to-noise ratio. This is not audio SNR; adjacent stations and non-uniform noise can affect the result. The reading may be unavailable when reference bands are invalid or significant clipping occurs.
- **I/Q total · dBFS:** overall captured energy before channel selection. Other stations within the capture can increase this value.

Levels are relative to digital full scale and are not calibrated in dBm. There is no universal I/Q level that guarantees good reception.

## MPX modulation and power

MPX is the demodulated composite signal before de-emphasis and stereo decoding. It includes audio, the pilot and RDS.

| Reading | What it displays |
| --- | --- |
| **MPX 60 s** | Moving average power over 60 seconds, in dBr |
| **MPX 1 s** | One-second average power, updated after each complete second |
| **+Mod / −Mod** | Positive and negative extremes within each 100 ms block, in kHz and percent |
| **Peak** | Largest absolute value of either polarity over the last second |
| **Pilot %** | Spectral level of the 19 kHz pilot, expressed as an equivalent percentage of ±75 kHz |
| **RDS % eq.** | Energy from 54–60 kHz converted to an equivalent peak percentage |
| **RMS** | RMS MPX deviation within a 100 ms block |
| **Max + / −** | Extremes by polarity since the session started |
| **RDS kHz RMS** | RMS level integrated over the 54–60 kHz band |

**100% modulation corresponds to ±75 kHz.** The **0 dBr** power reference is a sine wave producing ±19 kHz peak deviation, equivalent to approximately **13.44 kHz RMS**. Power averages are calculated in linear units.

RMS and 100 ms extremes are not 60-second averages. A high reading in a short block does not, by itself, determine the long-term window result.

The equivalent RDS level includes noise and interference in that band; it is not the actual peak of the modulated RDS signal or an error rate. Pilot and RDS levels are spectral estimates.

## Graphs

- **RF spectrum:** I/Q capture displayed in dBFS per bin, with a tuned-channel marker. During actual reception, the frequency axis uses MHz; Demo uses offsets in kHz.
- **MPX spectrum:** 0–60 kHz, with L+R, L−R, 19 kHz pilot and 57 kHz RDS references.
- **MPX Multiplex:** time-domain composite waveform/envelope in 50 ms windows, with ±75 kHz references. Select it under **Settings → MPX graph**.
- **MPX power:** 120 seconds of history showing 100 ms power and the 60-second moving average.
- **Audio L/R:** RMS and peak meters for the left and right channels.

In both RF and MPX spectra, **Min.** adjusts the lower display limit from **−120 to −40 dB**, in 10 dB steps. Limits are independent and saved. This changes only the visual scale; it does not increase gain or alter measurements.

Graphs can receive up to 20 frames per second; actual smoothness depends on the computer and Windows.

## Listen to the station

1. Select the sound device under **Settings → Audio output**.
2. Start reception.
3. Enable **Listen** in the main bar and adjust the volume.
4. Use **Mute** to stop listening without stopping measurements.

Output can use the Windows default or a selected sound device. It can be changed while listening. Volume does not affect MPX power or the Windows master volume.

Audio uses 48 kHz stereo with 50 µs de-emphasis. Stereo status depends on pilot synchronisation; without synchronisation, the application provides mono audio on both channels.

## Settings, gain and saved configuration

The gear icon provides source, USB device, audio output, gain/AGC mode, manual gain, PPM correction, RF filter, MPX view, language and automatic start settings.

- **Gain:** Manual, Tuner AGC, RTL AGC or Both AGC. The supplied tuner's maximum manual gain is 49.6 dB; the application selects a value supported by the device.
- **AGC:** adjusts gain automatically; it does not guarantee better SNR or provide additional RF gain above the manual maximum.
- **RF filter:** 320 or 280 kHz. The narrower option may help with adjacent channels, but also changes the response to wide modulation.
- **PPM:** corrects the receiver's frequency error.
- **Language:** Portuguese (Portugal), the default, or English (International). Received station text retains its original content.
- **Start reception on opening:** restores and starts the last saved source; disabled by default.

Configuration is created in **settings.xml** beside the executable and saved when closing Settings or the application. It includes tuning, gain, audio, view, scales and preferences. The previous configuration is kept in **settings.xml.bak**.

If the saved USB device is unavailable, the application waits for a new selection. When updating to another folder, copy the previous settings.xml to retain preferences. Do not share this file if you do not want to disclose your personal configuration.

## CSV logging and diagnostics

**Record CSV…** starts a measurement and RDS log, approximately one row per second. **Close CSV** stops logging. Demo mode is identified in the log. Logged short-term fields represent the selected measurement block, not every 100 ms block.

The footer displays:

- **I/Q:** overall digital level.
- **Clipping:** percentage of I/Q pairs near ADC limits; 0% is possible and does not rule out analogue tuner overload.
- **DSP:** processing time relative to signal duration; this is not overall CPU usage.
- **Window:** progress towards the complete 60-second window.

## Troubleshooting

| Situation | What to check |
| --- | --- |
| Application does not open or requests .NET | Install .NET 8 Desktop Runtime **x64** and extract the entire ZIP |
| RTL-SDR is not listed | USB connection, device selection, WinUSB on Interface 0 and **Find USB** |
| USB busy / error when starting | Close other SDR programs using the receiver |
| Weak or noisy signal | Antenna, cable, frequency and gain/AGC; maximum gain does not always give the best reception |
| No sound | Reception started, **Listen** enabled, volume, selected sound device and Windows mixer |
| Incomplete RDS | Wait for more groups, improve reception and confirm the station transmits RDS |
| MPX remains provisional | Allow 60 seconds without restarting tuning/gain |
| Readings differ from SDR# | Compare gain, AGCs, filters, FFT and references; dBFS/bin is not total power |
| Preferences are not saved | Use a folder with write permission |

## Scope and licences

The application is intended for FM monitoring. Noise, multipath, adjacent channels, overload and filters can affect results. Graph zoom does not calibrate the receiver, and a complete 60-second window does not certify broadcast compliance.

The **Yps Broadcast** name does not constitute an Authenticode digital signature. The minimal package includes notices and licences in **LICENSE**. This repository distributes the executable and documentation; the application's source code is not published here.
