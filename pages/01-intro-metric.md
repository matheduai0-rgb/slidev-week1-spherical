---
layout: center
class: text-center
---

<div class="text-left max-w-4xl mx-auto">

<div class="title-kicker">

MA3042 Geometri — Minggu 1 (Kuliah 1 & 2)

</div>

<div class="title-main">

Geometri Bola $\mathbb{S}^2$ dan Struktur Grup Isometri $\operatorname{Iso}(\mathbb{S}^2)$

</div>

<div class="title-sub">

Metrik Spherical, Garis Ekuidistan $G_{p_1, p_2}$, Teorema Tiga Refleksi, Basis Ortonormal $\{P,Q,\eta\}$, & Isomorfisma $O(3)$

</div>

<div class="title-meta">

**Rizal Afgani**  
Rujukan Utama: **Nam-Hoon Lee**, *Geometry: From Isometries to Special Relativity* (Bab 2: Subbab 2.1, 2.2, 2.4)  
Formulasi Analitik Basis & Refleksi: **Patrick J. Ryan**, *Euclidean and Non-Euclidean Geometry* (Bab 1 & 3)  
Fondasi Logika Deduktif: **Marvin Jay Greenberg**, *Euclidean and Non-Euclidean Geometries* (Bab 2)

</div>

</div>

---

# Peta Perkuliahan Minggu 1 & Cetak Biru Unifikasi Lee

<div class="grid grid-cols-12 gap-4 items-start">

<div class="col-span-5 text-xs">

- **Kuliah 1 (Lee Subbab 2.1): Metrik & Garis Spherical**
  - Jarak tali busur $d(p_1, p_2)$ vs. jarak intrinsik $d_{\mathbb{S}^2}(p_1, p_2)$
  - Lema 1 (Jembatan trigonometri) & Proposisi 2.2 (restriksi isometri $\mathbb{R}^3$)
  - Proposisi 2.3 ($G_{p_1, p_2}$ adalah lingkaran besar), Lemma 2.4, & Teorema Tiga Titik (Teorema 2.5)
- **Kuliah 2 (Lee 2.2, 2.4 & Ryan): Refleksi, Basis Ortonormal, & $O(3)$**
  - Refleksi $\bar{r}_G$, Teorema Tiga Refleksi (Teorema 2.7)
  - Basis ortonormal $\{u_1,u_2,u_3\}$, matriks transisi $P \in O(3)$, bentuk kanonik $\operatorname{diag}(1,1,-1)$, & matriks Householder
  - Formulasi Ryan $\{P,Q,\eta\}$, matriks $\bar{r}_\alpha \bar{r}_\beta$, Teorema Rotasi Euler (Teorema 2.8), & $\Psi: O(3) \stackrel{\sim}{\to} \operatorname{Iso}(\mathbb{S}^2)$ (Teorema 2.18)

</div>

<div class="col-span-7">

| Fitur Geometri | Bidang Euklides ($\mathbb{E}^2$) | Bola Satuan ($\mathbb{S}^2$) | Bidang Hiperbolik ($\mathbb{H}^2$) |
| :--- | :--- | :--- | :--- |
| **Himpunan Dasar** | $\mathbb{R}^2 = \{(x,y)\}$ | $\mathbb{S}^2 = \{p \in \mathbb{R}^3 \mid \Vert p \Vert = 1\}$ | $\mathbb{H}^2 = \{(x,y) \in \mathbb{R}^2 \mid y > 0\}$ |
| **Definisi Garis** | Ekuidistan $L_{p_1, p_2}$ | Ekuidistan $G_{p_1, p_2}$ | Ekuidistan $H_{p_1, p_2}$ (Def. 4.13) |
| **Wujud Garis** | Garis lurus di $\mathbb{R}^2$ | Lingkaran besar $\mathbb{S}^2 \cap P$ (Prop. 2.3) | Sinar vertikal / setengah lingkaran $\perp$ sumbu-$x$ |
| **Generator Atomik** | Refleksi garis $\bar{r}_L$ | Refleksi lingkaran besar $\bar{r}_G$ | Inversi hiperbolik $I_C$ (Thm. 4.4) |
| **Teorema Kekakuan** | Teorema 3 Titik (Thm. 1.5) | Teorema 3 Titik (Thm. 2.5) | Teorema 3 Titik (Thm. 4.19) |
| **Teorema Struktur** | Teorema 3 Refleksi (Thm. 1.6) | Teorema 3 Refleksi (Thm. 2.7) | Teorema 3 Inversi (Thm. 4.20) |

</div>

</div>

---

# Kuliah 1: Bola Satuan $\mathbb{S}^2$ dalam $\mathbb{R}^3$ & Metrik (Lee 2.1)

<div class="grid grid-cols-12 gap-4 items-center">

<div class="col-span-8">

<div class="def-box">

**Definisi (Bola Satuan $\mathbb{S}^2$).** Permukaan bola satuan di dalam ruang Euklides $\mathbb{R}^3$ didefinisikan sebagai:

$$\mathbb{S}^2 = \{ p \in \mathbb{R}^3 \mid |p| = 1 \} = \{ (x, y, z) \in \mathbb{R}^3 \mid x^2 + y^2 + z^2 = 1 \}.$$

</div>

<div v-click class="thm-box">

**Jarak Tali Busur ($\mathbb{R}^3$) vs. Jarak Spherical Intrinsik ($\mathbb{S}^2$):**

- **Jarak Tali Busur (Chordal Distance):** Untuk $p_1, p_2 \in \mathbb{S}^2$,
  $$d(p_1, p_2) = |p_1 - p_2| = \sqrt{|p_1|^2 - 2\langle p_1, p_2 \rangle + |p_2|^2} = \sqrt{2 - 2\langle p_1, p_2 \rangle}.$$
- **Jarak Spherical Intrinsik $d_{\mathbb{S}^2}$:** Diukur sepanjang busur lingkaran besar (sudut pusat $\angle p_1 0 p_2 \in [0, \pi]$):
  $$d_{\mathbb{S}^2}(p_1, p_2) = \arccos\langle p_1, p_2 \rangle \in [0, \pi].$$

</div>

</div>

<div class="col-span-4">

<JxgMetricChord height="255px" :showBisector="false" />

</div>

</div>

---

# Jembatan Trigonometri Fundamental: $d_{\mathbb{S}^2}$ vs $d$ (Lema 1)

<div class="grid grid-cols-12 gap-4 items-center">

<div class="col-span-8">

<div class="thm-box">

**Persamaan 2.1 Lee & Lema 1 (Bentuk Kanonik $H \Rightarrow C$).**

- **Hipotesis $H$:** $p_1, p_2, q_1, q_2 \in \mathbb{S}^2$ adalah titik-titik pada bola satuan.
- **Kesimpulan $C$:** Berlaku identitas trigonometri fundamental
  $$\sin\!\left(\tfrac{1}{2} d_{\mathbb{S}^2}(p_1, p_2)\right) = \tfrac{1}{2} d(p_1, p_2),$$
  dan kesamaan jarak tali busur ekuivalen dengan kesamaan jarak spherical:
  $$d(p_1, p_2) = d(q_1, q_2) \iff d_{\mathbb{S}^2}(p_1, p_2) = d_{\mathbb{S}^2}(q_1, q_2).$$

</div>

<div v-click class="proof-box">

**Bukti Formal Deduktif (Aturan Logika 1 Greenberg):**

1. Tinjau segitiga sama kaki $\triangle p_1 0 p_2$ di $\mathbb{R}^3$ dengan $|p_1| = |p_2| = 1$ dan sudut puncak $\angle p_1 0 p_2 = d_{\mathbb{S}^2}(p_1, p_2)$ <span class="just">(Definisi $\mathbb{S}^2$ & $d_{\mathbb{S}^2}$)</span>.
2. Garis bagi sudut dari titik asal $0$ membagi tegak lurus tali busur $p_1 p_2$ menjadi dua segmen sepanjang $\tfrac{1}{2}d(p_1, p_2)$ dengan sudut setengah $\theta = \tfrac{1}{2}d_{\mathbb{S}^2}(p_1, p_2)$, sehingga $\sin(\tfrac{1}{2}d_{\mathbb{S}^2}(p_1,p_2)) = \tfrac{1}{2}d(p_1,p_2)$ <span class="just">(Trigonometri Segitiga Siku-Siku)</span>.
3. Karena fungsi $t \mapsto 2\sin(t/2)$ **monoton naik tegas** dan bijektif pada $[0, \pi]$, maka $d(p_1, p_2) = d(q_1, q_2) \iff d_{\mathbb{S}^2}(p_1, p_2) = d_{\mathbb{S}^2}(q_1, q_2)$ <span class="just">(Sifat Fungsi Injektif pada $[0,\pi]$)</span>. <span class="qed">∎</span>

</div>

</div>

<div class="col-span-4">

<JxgMetricChord height="255px" :showBisector="true" />

</div>

</div>

---

# Proposisi 2.2: Isometri pada $\mathbb{S}^2$ & Restriksi Isometri $\mathbb{R}^3$

<div class="grid grid-cols-12 gap-4 items-center">

<div class="col-span-10">

<div class="def-box">

**Definisi 2.1 (Isometri pada $\mathbb{S}^2$ — Lee).** Pemetaan bijektif $\phi: \mathbb{S}^2 \to \mathbb{S}^2$ disebut **isometri pada $\mathbb{S}^2$** ($\phi \in \operatorname{Iso}(\mathbb{S}^2)$) jika mengawetkan jarak spherical untuk setiap $p_1, p_2 \in \mathbb{S}^2$:

$$d_{\mathbb{S}^2}(\phi(p_1), \phi(p_2)) = d_{\mathbb{S}^2}(p_1, p_2).$$

</div>

<div v-click class="thm-box">

**Proposisi 2.2 (Lee — Bentuk Kanonik $H \Rightarrow C$).**

- **Hipotesis $H$:** $\phi: \mathbb{R}^3 \to \mathbb{R}^3$ adalah isometri Euklides pada $\mathbb{R}^3$ yang mengawetkan titik asal $0$ ($\phi(0) = 0$).
- **Kesimpulan $C$:** Restriksi $\psi = \phi|_{\mathbb{S}^2}: \mathbb{S}^2 \to \mathbb{S}^2$ merupakan suatu isometri pada $\mathbb{S}^2$.

</div>

<div v-click class="proof-box">

**Bukti Formal Deduktif (Aturan Logika 1 Greenberg):**

1. Ambil sebarang $p \in \mathbb{S}^2$, maka $d(0, p) = |p| = 1$ <span class="just">(Definisi $\mathbb{S}^2$)</span>.
2. Karena $\phi$ isometri $\mathbb{R}^3$ dan $\phi(0) = 0$, maka $|\phi(p)| = d(0, \phi(p)) = d(\phi(0), \phi(p)) = d(0, p) = 1$, sehingga $\psi(p) = \phi(p) \in \mathbb{S}^2$ <span class="just">(Hipotesis $H$ & Definisi Isometri $\mathbb{R}^3$)</span>.
3. Untuk setiap $p_1, p_2 \in \mathbb{S}^2$, berlaku $d(\psi(p_1), \psi(p_2)) = d(p_1, p_2)$ <span class="just">(Hipotesis $H$)</span>.
4. Berdasarkan Lema 1, diperoleh $d_{\mathbb{S}^2}(\psi(p_1), \psi(p_2)) = d_{\mathbb{S}^2}(p_1, p_2)$ <span class="just">(Langkah 3 & Lema 1)</span>. <span class="qed">∎</span>

</div>

</div>



</div>
