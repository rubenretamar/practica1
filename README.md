# Práctica 01 · Misión presupuestos

**Sistemas Informáticos · 1º DAM** · Rubén Argudo · Precios consultados el 15/11/2026 en Amazon.es (IVA incluido) y contrastados con PcComponentes.

## 0. Resumen y contexto de mercado

| Perfil | Base | Bono asignado | **Total** | Consumo máx. (BuildCores) | Build pública |
|---|---:|---:|---:|---:|---|
| 1 · Desarrolladora Junior | 1.000 € | +464,22 € | **1.464,22 €** | 279 W | [BuildCores](https://www.buildcores.com/builds/0YKaABwo5?share=true) |
| 2 · Técnico de Datos IA | 1.000 € | +953,50 € | **1.953,50 €** | 369 W | [BuildCores](https://www.buildcores.com/builds/cWhvskqFr?share=true) |
| 3 · Administración | 1.000 € | 0 € (sobran 90,66 €) | **909,34 €** | 159 W | [BuildCores](https://www.buildcores.com/builds/ULiQ4kx95?share=true) |
| **Total** | | **1.417,72 € de 2.000 €** | **4.327,06 €** | | **Ahorro: 672,94 €** |

Las tres builds salen **"Compatible"** en BuildCores (BuildCores muestra precios de tiendas de EE. UU. en $; los precios válidos son los de este informe).

**La decisión que lo condiciona todo: plataforma DDR4.** En noviembre de 2026 la RAM DDR5 está disparada por la demanda de memoria para centros de datos de IA: un kit Kingston FURY Beast DDR5-6000 32 GB (2×16) cuesta [757,85 € en PcComponentes](https://www.pccomponentes.com/buscar/?query=kingston%20fury%20beast%2032gb%20ddr5%206000) y la prensa habla de [más de un +400 % en un año](https://www.profesionalreview.com/2026/10/14/ddr5-precios-aumentaron-500-en-un-ano/amp/). Un kit DDR4-3200 CL16 de 32 GB cuesta [259,99 €](https://www.amazon.es/dp/B07RW6Z692). Un Ryzen 9 9900X (AM5) cuesta solo [317,87 €](https://www.amazon.es/dp/B0D6NN87T8), pero obliga a DDR5: **+≈500 € por equipo** solo en memoria. Por eso los tres equipos usan AM4 o LGA1700 con DDR4 y el dinero va a donde el perfil lo nota.

---

## 1. Desarrolladora Junior · VS Code, Git, Linux · 1.464,22 €

| Componente | Modelo | Dato clave de la ficha | Precio |
|---|---|---|---:|
| CPU | [AMD Ryzen 9 5900XT](https://www.amazon.es/dp/B0GSVS85Z9) | 16 núcleos / 32 hilos, todos iguales; 64 MB L3; 105 W | 367,43 € |
| Disipador | [Thermalright Peerless Assassin 120 SE](https://www.amazon.es/dp/B0CKP85L5C) | Doble torre, 2×120 mm, 155 mm de alto | 45,90 € |
| Placa | [ASUS TUF Gaming B550M-Plus WiFi II](https://www.amazon.es/dp/B09LMQ3ZW8) | AM4, 4 DIMM, 2× M.2, PCIe 4.0 | 134,95 € |
| RAM | [Corsair Vengeance LPX 32 GB (2×16) DDR4-3200 CL16](https://www.amazon.es/dp/B07RW6Z692) | Dual channel, 51,2 GB/s | 259,99 € |
| SSD | [WD_Black SN7100 1 TB](https://www.amazon.es/dp/B0DN7CYYSD) | NVMe PCIe 4.0 x4, 7.250/6.900 MB/s, TLC | 198,58 € |
| Gráfica | [Radeon RX 550 4 GB (MAXSUN)](https://www.amazon.es/dp/B08VHWFWSD) | DP + HDMI + DVI-D DL; driver libre `amdgpu` en Linux | 121,98 € |
| Fuente | [Corsair CX550 80+ Bronze](https://www.amazon.es/dp/B0CJRVGGPS) | 550 W; consumo estimado 279 W (51 % de carga) | 54,99 € |
| Caja | [Kolink Citadel Mesh](https://www.amazon.es/dp/B08D6PLW2K) | micro-ATX, frontal de malla | 59,00 € |
| Monitores | 2× [AOC Q27B35S3](https://www.amazon.es/dp/B0F8NQQTKP) | 27" IPS 2560×1440, 120 Hz | 198,00 € |
| Teclado + ratón | [Logitech MK295 Silent (ES)](https://www.amazon.es/dp/B07W7KGYN1) | Inalámbrico, silencioso | 23,40 € |
| SO | Ubuntu 24.04 LTS | Gratuito | 0,00 € |

**Justificación con datos**

- **CPU (requisito: multicore).** Compilar un proyecto grande con `make -j32` escala con núcleos reales. Ryzen 9 5900XT: [27.966 pts en Cinebench R23 multi](https://www.cpu-monkey.com/en/cpu-AMD_Ryzen_9_5900XT) frente a [21.568 del i7-12700](https://www.cpu-monkey.com/en/compare_cpu-amd_ryzen_9_5900-vs-intel_core_i7_12700f) (+30 %). Para las VMs, los 16 núcleos son idénticos: 3 VMs × 4 vCPU = 12 hilos y quedan 20 para el anfitrión y los contenedores, sin el riesgo de que el hipervisor (KVM) asigne vCPUs a E-cores lentos.
- **RAM 32 GB (2×16, dual channel):** 3 VMs × 4 GB = 12 GB + Ubuntu ~2,5 GB + VS Code ~1,5 GB + navegador ~2 GB + Docker (BD + backend) ~3 GB + compilación paralela ~4 GB ≈ **25 GB** → 32 GB con margen. 16 GB obligarían a usar swap.
- **SSD TLC NVMe:** las imágenes de las VMs (3 × 40 GB) y las capas de Docker generan mucha escritura aleatoria; TLC aguanta mejor las escrituras sostenidas que una QLC.
- **Gráfica:** el 5900XT no tiene gráfica integrada, así que hace falta una. La RX 550 es la más barata que encontré con DP + HDMI para dos monitores 1440p y con driver libre en el kernel. Una GT 710 (61,99 €) solo saca 1440p por HDMI 1.4, y NVIDIA ya la tiene en el driver legacy 470.
- **Bono (+464 €):** 32 GB de RAM y un 2.º monitor. Dos pantallas de 27" QHD permiten tener el IDE, la terminal/VM y la documentación a la vez.

**Cuello de botella:** compilando, manda la CPU, y en el enlazado el ancho de banda DDR4 (51,2 GB/s para 16 núcleos). Pega honesta: AM4 es una plataforma sin futuro. Se acepta porque ahorra unos 500 € en memoria.

---

## 2. Técnico de Datos IA · TensorFlow, PyTorch, LLM ~14B · 1.953,50 €

| Componente | Modelo | Dato clave de la ficha | Precio |
|---|---|---|---:|
| GPU | [ASUS Dual RTX 5060 Ti **16 GB** OC](https://www.amazon.es/dp/B0F4DVKSZ4) | 16 GB GDDR7, 128 bit, **448 GB/s**, **180 W**, PCIe 5.0 x8, CUDA | 849,95 € |
| CPU | [AMD Ryzen 5 5600](https://www.amazon.es/dp/B09VCHR1VH) | 6C/12H, 65 W, **PCIe 4.0** | 152,59 € |
| Disipador | [ARCTIC Freezer 36](https://www.amazon.es/dp/B09VG6NBSJ) | Torre 2×120 mm, más silencioso que el de serie | 27,73 € |
| Placa | [ASUS Prime B550M-A WiFi II](https://www.amazon.es/dp/B09LVQXHBG) | PCIe 4.0 en la ranura de la GPU y en el M.2 | 99,95 € |
| RAM | [Corsair Vengeance LPX 32 GB (2×16) DDR4-3200 CL16](https://www.amazon.es/dp/B07RW6Z692) | Dual channel | 259,99 € |
| SSD | [Kingston NV3 2 TB](https://www.amazon.es/dp/B0DBR6TRZQ) | NVMe PCIe 4.0 | 274,95 € |
| Fuente | [be quiet! Pure Power 12 650 W](https://www.amazon.es/dp/B0DLH776YJ) | 80+ Gold; 369 W estimados = 57 % de carga | 84,99 € |
| Caja | [Fractal Design Pop Air](https://www.amazon.es/dp/B08SR7LPCD) | ATX, 3 ventiladores, mucho flujo de aire a pocas RPM | 80,95 € |
| Monitor | [AOC Q27B35S3](https://www.amazon.es/dp/B0F8NQQTKP) | 27" QHD IPS | 99,00 € |
| Teclado + ratón | [Logitech MK295 Silent](https://www.amazon.es/dp/B07W7KGYN1) | | 23,40 € |
| SO | Ubuntu 24.04 LTS | Gratuito; TensorFlow no tiene GPU en Windows nativo desde la 2.11 | 0,00 € |

**Justificación con datos**

- **¿Qué acelera la IA local: GPU, NPU o memoria? → la memoria de la GPU.** Un 14B en Q4_K_M ocupa [9,0 GB en Ollama](https://ollama.com/library/qwen2.5:14b-instruct-q4_K_M/blobs/2049f5674b1e). Para cada token generado hay que leer el modelo entero, así que el límite teórico es ancho de banda ÷ tamaño: **GPU 448 / 9 ≈ 50 tokens/s** y **CPU con DDR4 dual 51,2 / 9 ≈ 5,7 tokens/s**. La GPU es casi 9 veces más rápida. Los 16 GB caben el modelo (~9 GB) más 8–32k de contexto (~11 GB en total).
- **Restricción "consumo y ruido":** la 5060 Ti tiene [180 W de TGP y un único conector de 8 pines](https://www.techspot.com/specs/gpu/307872-nvidia-geforce-rtx-5060-ti-16gb.html). Una RTX 5070 consume 250 W y trae menos VRAM (12 GB). Generar texto está limitado por la memoria, no por el cálculo, así que se puede bajar el límite con `nvidia-smi -pl 150` y perder muy poco. Con la fuente Gold trabajando cerca del 50 %, la caja de 3 ventiladores y el ventilador de la gráfica que se para en reposo, el equipo hace poco ruido durante horas.
- **CPU modesta a propósito:** la CPU no participa en la inferencia. Un 5600 (sin G) tiene **PCIe 4.0**, y la 5060 Ti, que solo usa 8 carriles, va a 4.0 x8 (~16 GB/s). Con un 5600G/5700G (solo PCIe 3.0) bajaría a ~8 GB/s.
- **RAM 32 GB:** Ubuntu ~2,5 GB + contexto CUDA de PyTorch/TF ~3–4 GB + datasets en pandas o DataLoader ~8–12 GB + caché del modelo al cargarlo (~9 GB) ≈ **25 GB**.
- **SSD 2 TB:** varios modelos de ~9 GB, imágenes Docker con CUDA (~10–20 GB cada una) y datasets. Es el perfil que más disco gasta.
- **Bono (+953,50 €):** casi todo va a la GPU de 16 GB, que es lo que este perfil nota.

**Cuello de botella:** la VRAM. Un modelo de 32B (~20 GB) no cabría y tendría que repartirse entre GPU y RAM, y la velocidad caería hacia la de la DDR4.

---

## 3. Administración · Office 365, ERP, BI · 909,34 €

| Componente | Modelo | Dato clave de la ficha | Precio |
|---|---|---|---:|
| CPU | [Intel Core i5-12400 (caja, con disipador)](https://www.amazon.es/dp/B09NMPD8V2) | 6 P-cores / 12 hilos, **0 E-cores**; gráfica UHD 730; disipador de serie | 153,36 € |
| Placa | [Gigabyte B760M D3HP DDR4](https://www.amazon.es/dp/B0D6W46N73) | LGA1700, DDR4, M.2 PCIe 4.0 | 85,98 € |
| RAM | [Patriot Viper Steel 16 GB (2×8) DDR4-3200 CL16](https://www.amazon.es/dp/B07KXMWHBY) | Dual channel | 139,99 € |
| SSD | [Lexar NQ790 1 TB](https://www.amazon.es/dp/B0CHRX3TKK) | NVMe PCIe 4.0 x4, 7.000/6.000 MB/s | 159,99 € |
| Fuente | [Corsair CX550 80+ Bronze](https://www.amazon.es/dp/B0CJRVGGPS) | 159 W estimados | 54,99 € |
| Caja | [NZXT H3 Flow](https://www.amazon.es/dp/B0DQPMJ6MJ) | micro-ATX, frontal de malla | 58,00 € |
| Monitor | [Philips 24E1N1100A](https://www.amazon.es/dp/B0CX9CT3ZF) | 24" IPS FHD 120 Hz, **altavoces integrados** | 69,00 € |
| Teclado + ratón | [Logitech MK295 Silent](https://www.amazon.es/dp/B07W7KGYN1) | Silencioso, para oficina | 23,40 € |
| Webcam | [Logitech C270](https://www.amazon.es/dp/B01BGBJ8Y0) | 720p con micrófono, para videollamadas | 14,99 € |
| SO | [Windows 11 Pro OEM](https://www.amazon.es/dp/B09HM7FG2Y) | Unirse a dominio/Entra ID y BitLocker | 149,64 € |

**Justificación con datos**

- **"Ni un euro de más":** sin gráfica dedicada (la UHD 730 basta para ofimática y BI), con el disipador que viene en la caja y sin pedir bono. Sobran 90,66 € de la base.
- **CPU de solo P-cores:** el ERP, Excel y el navegador funcionan con pocos hilos rápidos. Seis núcleos iguales evitan que Windows mande una videollamada a un E-core.
- **RAM 16 GB (2×8, dual channel):** Windows 11 ~4 GB + 40 pestañas en Edge con pestañas suspendidas ~4–5 GB + Teams ~1,5 GB + Office ~1,5 GB + ERP ~1 GB + Power BI ~1,5 GB ≈ **14 GB**. 32 GB costarían unos 120 € más y no se notarían.
- **Almacenamiento (requisito clave):** 1 TB NVMe para la caché de Outlook (OST), la sincronización local de OneDrive y los archivos `.pbix` de Power BI. Cuesta menos que un M.2 SATA (ver trampa 1).
- **Windows 11 Pro y no Home:** solo cuesta 4,64 € más ([Home a 145 €](https://www.amazon.es/dp/B09XH2JMMS)) y es la versión que puede unirse al dominio o a Entra ID, cosa que una empresa con ERP necesita.
- **Videollamadas sin auriculares:** la webcam lleva micrófono y el monitor lleva altavoces. Son 14,99 € en total.

**Cuello de botella:** la RAM si se trabaja con modelos de Power BI muy grandes. Se puede ampliar cambiando a 2×16 sin tocar nada más.

---

## 4. Trampas del mercado detectadas

| Trampa | Ejemplo real (15/11/2026) | Cómo la evito |
|---|---|---|
| **1 · El falso rápido** (M.2 con bus SATA) | [WD Blue SA510 M.2 **SATA** 1 TB: 560 MB/s por 179,99 €](https://www.amazon.es/dp/B09ZYNHPW2) frente a [Lexar NQ790 M.2 **NVMe**: 7.000 MB/s por 159,99 €](https://www.amazon.es/dp/B0CHRX3TKK). Es 12,5 veces más rápido y cuesta 20 € menos. | Los tres SSD son **NVMe PCIe 4.0 x4**: lo compruebo en la ficha, no me fío del conector. |
| **2 · La ilusión óptica** (muchos núcleos que son E-cores) | El Core Ultra 5 250KF Plus se vende como "18 núcleos" ([214,70 € en PcComponentes](https://www.pccomponentes.com/buscar/?query=core%20ultra%205%20250kf%20plus)), pero 12 son E-cores. El [i5-14400](https://www.amazon.es/dp/B0CQ3HKJYV) tiene "10 núcleos" = 6P + 4E. | Dev: 16 núcleos completos (5900XT). Admin: 6 P-cores y 0 E-cores (i5-12400). |
| **3 · El falso ahorro energético** (NPU con muchos TOPS) | El [Ryzen 5 8600G](https://www.amazon.es/dp/B0CQ4GYTTX) anuncia una NPU de 16 TOPS, pero [Ollama y llama.cpp funcionan con CPU y GPU, y la NPU se queda sin usar](https://www.buildmvpfast.com/blog/npu-local-ai-development-reality-check-2026). Además es AM5 y obliga a DDR5. | GPU con 16 GB a 448 GB/s: lo que manda es el ancho de banda de la memoria. |
| **Extra · 8 GB o 16 GB con el mismo nombre** | La RTX 5060 Ti también existe con 8 GB: ahí el 14B (9 GB) no cabe. | Elegida expresamente la versión de **16 GB**. |
| **Extra · DDR5 en 2026** | 32 GB de DDR5 ≈ 758 € frente a 32 GB de DDR4 a 259,99 €. | Plataformas DDR4 (AM4 / B760 DDR4). |

## 5. Fuentes
Precios: Amazon.es (enlaces en cada tabla) y PcComponentes, consultados el 15/11/2026. Fichas y rendimiento: [TechSpot – RTX 5060 Ti 16 GB](https://www.techspot.com/specs/gpu/307872-nvidia-geforce-rtx-5060-ti-16gb.html) · [CPU-Monkey – Ryzen 9 5900XT](https://www.cpu-monkey.com/en/cpu-AMD_Ryzen_9_5900XT) · [CPU-Monkey – i7-12700F](https://www.cpu-monkey.com/en/compare_cpu-amd_ryzen_9_5900-vs-intel_core_i7_12700f) · [Ollama – qwen2.5:14b Q4_K_M (9,0 GB)](https://ollama.com/library/qwen2.5:14b-instruct-q4_K_M/blobs/2049f5674b1e) · [Profesional Review – precio DDR5](https://www.profesionalreview.com/2026/10/14/ddr5-precios-aumentaron-500-en-un-ano/amp/) · [Estado de las NPU en IA local (2026)](https://www.buildmvpfast.com/blog/npu-local-ai-development-reality-check-2026). Compatibilidad: BuildCores (enlaces en la sección 0).
