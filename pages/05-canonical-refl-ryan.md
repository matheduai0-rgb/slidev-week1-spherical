# Teorema 5: Bentuk Kanonik Diagonal Matriks Refleksi Bola

<div class="grid grid-cols-12 gap-4 items-center">

<div class="col-span-11">

<div class="thm-box">

**Teorema 5 (Bentuk Kanonik Diagonal Refleksi Bola — Lee & Ryan).**

- **Hipotesis $H$:** $G = \mathbb{S}^2 \cap P$ adalah lingkaran besar bervektor kutub normal satuan $u_3$, dan $\mathcal{B} = \{u_1, u_2, u_3\}$ adalah basis ortonormal teradaptasi bagi $\mathbb{R}^3$ ($u_1, u_2 \in G$).
- **Kesimpulan $C$:** Matriks representasi refleksi bola $\bar{r}_P$ dalam basis $\mathcal{B}$ berbentuk diagonal kanonik murni:
  $$[\bar{r}_P]_{\mathcal{B}} = \begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & -1 \end{bmatrix} = \operatorname{diag}(1, 1, -1).$$

</div>

<div v-click class="proof-box">

**Bukti :**

1. Kolom ke-$j$ dari matriks representasi $[T]_{\mathcal{B}}$ adalah vektor koordinat $[T(u_j)]_{\mathcal{B}}$ <span class="just">(Definisi Matriks Representasi Pemetaan Linier)</span>.
2. **Kolom 1 & 2:** Karena $u_1, u_2 \in P = \operatorname{Fix}(\bar{r}_P)$, berlaku $\bar{r}_P(u_1) = u_1 \implies [\bar{r}_P(u_1)]_{\mathcal{B}} = (1,0,0)^T$ dan $\bar{r}_P(u_2) = u_2 \implies [\bar{r}_P(u_2)]_{\mathcal{B}} = (0,1,0)^T$ <span class="just">(Sifat Titik Tetap)</span>.
3. **Kolom 3:** Karena $u_3 \perp P$, $\bar{r}_P(u_3) = -u_3 \implies [\bar{r}_P(u_3)]_{\mathcal{B}} = (0,0,-1)^T$ <span class="just">(Refleksi Vektor Normal)</span>.
4. Gabungkan ketiga kolom: $[\bar{r}_P]_{\mathcal{B}} = \operatorname{diag}(1, 1, -1)$ <span class="just">(Langkah 2 & 3)</span>. <span class="qed">∎</span>

</div>

<div v-click class="socratic-box">

**Wawasan Intrinsik:** Setiap refleksi lingkaran besar pada $\mathbb{S}^2$ secara intrinsik selalu identik dengan refleksi ekuator $(x_1, x_2, x_3) \mapsto (x_1, x_2, -x_3)$ bila dipandang dari koordinat basis ortonormalnya sendiri!

</div>

</div>



</div>

---

# Teorema 6: Penurunan Matriks Householder via Perubahan Basis

<div class="grid grid-cols-12 gap-4 items-center">

<div class="col-span-12">

<div class="thm-box">

**Teorema 6 (Penurunan Matriks Householder $[\bar{r}_P]_{\mathcal{E}} = I_3 - 2 u_3 u_3^T$).**

- **Hipotesis $H$:** $\mathcal{B} = \{u_1, u_2, u_3\}$ adalah basis ortonormal teradaptasi pada $G = \mathbb{S}^2 \cap P$ dengan kutub normal satuan $u_3$.
- **Kesimpulan $C$:** Dalam basis standar $\mathcal{E}$, matriks refleksi $[\bar{r}_P]_{\mathcal{E}}$ hanya bergantung pada kutub $u_3$:
  $$[\bar{r}_P]_{\mathcal{E}} = I_3 - 2 u_3 u_3^T, \quad \det(\bar{r}_P) = -1, \quad \operatorname{tr}(\bar{r}_P) = 1.$$

</div>

<div v-click class="proof-box">

**Bukti Formal Deduktif (Aturan Logika 1 Greenberg):**

1. Terapkan rumus perubahan basis balik $[T]_{\mathcal{E}} = P [T]_{\mathcal{B}} P^T$ dengan $P = [u_1 \ u_2 \ u_3] \in O(3)$ <span class="just">(Proposisi 4 & Teorema 5)</span>:
   $$[\bar{r}_P]_{\mathcal{E}} = [u_1 \ u_2 \ u_3] \begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & -1 \end{bmatrix} \begin{bmatrix} u_1^T \\ u_2^T \\ u_3^T \end{bmatrix} = u_1 u_1^T + u_2 u_2^T - u_3 u_3^T.$$
2. Dari identitas kelengkapan $P P^T = I_3 \iff u_1 u_1^T + u_2 u_2^T + u_3 u_3^T = I_3$, substitusikan $u_1 u_1^T + u_2 u_2^T = I_3 - u_3 u_3^T$:
   $$[\bar{r}_P]_{\mathcal{E}} = (I_3 - u_3 u_3^T) - u_3 u_3^T = I_3 - 2 u_3 u_3^T \quad .$$
3. Invarian spektral bebas basis: $\det(\bar{r}_P) = \det([\bar{r}_P]_{\mathcal{B}}) = (1)(1)(-1) = -1$ dan $\operatorname{tr}(\bar{r}_P) = 1 + 1 - 1 = 1$ <span class="just">(Keserupaan Matriks)</span>. <span class="qed">∎</span>

</div>

</div>


</div>

---

# Contoh Komputasi Eksplisit Matriks Refleksi ($P: x + y - \sqrt{2}z = 0$)

<div class="grid grid-cols-12 gap-4 items-center">

<div class="col-span-8">

<div class="def-box">

**Soal Komputasi:** Diberikan lingkaran besar $G = \mathbb{S}^2 \cap P$ dengan bidang cermin $P: x + y - \sqrt{2}z = 0$.

1. Tentukan vektor kutub normal satuan $u_3$ dan susun basis ortonormal teradaptasi $\mathcal{B} = \{u_1, u_2, u_3\}$.
2. Tentukan matriks kanonik $[\bar{r}_P]_{\mathcal{B}}$ dan hitung matriks refleksi standar $[\bar{r}_P]_{\mathcal{E}}$.

</div>

<div v-click class="proof-box">

**Penyelesaian Aljabar Langkah-demi-Langkah:**

1. Vektor normal $n = (1, 1, -\sqrt{2})^T$ memiliki panjang $|n| = \sqrt{1 + 1 + 2} = 2$, sehingga kutub normal satuan adalah $u_3 = \frac{1}{2}(1, 1, -\sqrt{2})^T$.
2. Pilih $u_1 \in P$ berjejari satuan: ambil $u_1 = \frac{1}{\sqrt{2}}(1, -1, 0)^T$ (jelas $\langle u_1, u_3 \rangle = 0$ dan $|u_1|=1$).
3. Bentuk $u_2 = u_3 \times u_1 = \frac{1}{2}(-1, -1, -\sqrt{2})^T$. Maka $\mathcal{B} = \{u_1, u_2, u_3\}$ ortonormal dengan $[\bar{r}_P]_{\mathcal{B}} = \operatorname{diag}(1, 1, -1)$.
4. Matriks Householder dalam basis standar $\mathcal{E}$:
   $$[\bar{r}_P]_{\mathcal{E}} = I_3 - 2 u_3 u_3^T = I_3 - \frac{1}{2}\begin{bmatrix} 1 & 1 & -\sqrt{2} \\ 1 & 1 & -\sqrt{2} \\ -\sqrt{2} & -\sqrt{2} & 2 \end{bmatrix} = \frac{1}{2}\begin{bmatrix} 1 & -1 & \sqrt{2} \\ -1 & 1 & \sqrt{2} \\ \sqrt{2} & \sqrt{2} & 0 \end{bmatrix}.$$
   Verifikasi: $\operatorname{tr}([\bar{r}_P]_{\mathcal{E}}) = \frac{1+1+0}{2} = 1$ dan $\det([\bar{r}_P]_{\mathcal{E}}) = -1$. <span class="qed">∎</span>

</div>

</div>

<div class="col-span-4">

<JxgOrthonormalBasis :presetExample="true" height="255px" />

</div>

</div>

---

# Formulasi Refleksi dalam Basis $\{P, Q, \eta\}$ (Patrick J. Ryan)

<div class="grid grid-cols-12 gap-4 items-center">

<div class="col-span-7">

<div class="def-box">

**Konstruksi Basis Ortonormal $\{P, Q, \eta\}$ pada Garis Ekuator $G$ (Ryan):** Misalkan $G \subset \mathbb{S}^2$ adalah lingkaran besar dengan kutub $\eta \in \mathbb{S}^2$ ($G = \mathbb{S}^2 \cap \eta^\perp$), dan pilih $P, Q \in G$ dengan $Q = \eta \times P$. Maka $\mathcal{B} = \{P, Q, \eta\}$ adalah basis ortonormal yang memenuhi aturan tangan kanan bagi $\mathbb{R}^3$, dan setiap titik pada $G$ dapat dinyatakan sebagai:

$$u(t) = (\cos t)P + (\sin t)Q \in \ell, \quad t \in [0, 2\pi).$$

</div>

<div v-click class="thm-box">

**Kutub Meridian $\alpha, \beta$ & Matriks Refleksi $\bar{r}_\alpha$ terhadap $\{P, Q, \eta\}$:** Misalkan lingkaran besar meridian $\alpha, \beta$ melalui kutub $\eta$ ($\alpha, \beta \perp \ell$) dan memotong $G$ di $u(a)$ serta $u(b)$. Maka:

- $N_\alpha = (-\sin a)P + (\cos a)Q$ adalah **kutub dari $\alpha$**, dan $N_\beta = (-\sin b)P + (\cos b)Q$ adalah **kutub dari $\beta$**.
- Terhadap basis ortonormal $\{P, Q, \eta\}$, matriks refleksi $\bar{r}_\alpha$ (dinotasikan $\Omega_\alpha$ di Ryan) adalah:
  $$[\bar{r}_\alpha]_{\mathcal{B}} = \begin{bmatrix} \cos 2a & \sin 2a & 0 \\ \sin 2a & -\cos 2a & 0 \\ 0 & 0 & 1 \end{bmatrix}.$$

</div>

</div>

<div class="col-span-5">

<JxgRyanMeridianRotation :showFourthLine="false" height="255px" />

</div>

</div>

---

# Teorema 7: Komposisi $\bar{r}_\alpha \bar{r}_\beta$ & Eksistensi Garis Keempat $\delta$

<div class="grid grid-cols-12 gap-4 items-center">

<div class="col-span-12">

<div class="thm-box">

**Teorema 7 (Matriks Komposisi Dua Refleksi $\bar{r}_\alpha \bar{r}_\beta$ terhadap $\{P, Q, \eta\}$ — Ryan).** Terhadap basis ortonormal $\{P, Q, \eta\}$, hasil kali refleksi $\bar{r}_\alpha$ dan $\bar{r}_\beta$ diberikan oleh:

$$\bar{r}_\alpha \bar{r}_\beta = \begin{bmatrix} \cos 2a & \sin 2a & 0 \\ \sin 2a & -\cos 2a & 0 \\ 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} \cos 2b & \sin 2b & 0 \\ \sin 2b & -\cos 2b & 0 \\ 0 & 0 & 1 \end{bmatrix} = \begin{bmatrix} \cos 2(a-b) & -\sin 2(a-b) & 0 \\ \sin 2(a-b) & \cos 2(a-b) & 0 \\ 0 & 0 & 1 \end{bmatrix} = \begin{bmatrix} \operatorname{rot} 2(a-b) & 0 \\ 0 & 1 \end{bmatrix}.$$

</div>

<JxgRyanMeridianRotation :showFourthLine="false" height="255px" />

</div>



</div>

---

# Diskusi Sokratik & Latihan Pengayaan Basis Ortonormal

<div class="socratic-box">

**Pertanyaan Sokratik:**

1. Mengapa bekerja pada basis ortonormal teradaptasi $\mathcal{B}$ membuat analisis geometri refleksi jauh lebih sederhana daripada menghitung langsung pada basis baku $\mathcal{E}$?
2. Jika kita meninjau komposisi dua refleksi bola $r = \bar{r}_{G_1} \circ \bar{r}_{G_2}$, basis ortonormal seperti apa yang membuat matriks $[r]_{\mathcal{B}}$ berbentuk blok rotasi $\operatorname{diag}(R_\theta, 1)$?

</div>

<div v-click class="exercise-box">

**Latihan Pengayaan (Basis Ortonormal & Matriks Householder):**

- **Latihan P.1:** Diberikan bidang cermin $P: x - z = 0$.
  - (a) Konstruksi basis ortonormal teradaptasi $\mathcal{B} = \{u_1, u_2, u_3\}$.
  - (b) Hitung matriks transisi $P$ dan verifikasi $P^T P = I_3$.
  - (c) Hitung matriks standar $[\bar{r}_P]_{\mathcal{E}} = P \operatorname{diag}(1, 1, -1) P^T$ dan bandingkan dengan rumus Householder $I_3 - 2 u_3 u_3^T$.
- **Latihan P.2 (Rotasi Bola via Basis Bersama):** Misalkan dua lingkaran besar $G_1, G_2$ berpotongan di sumbu $u_3 \in \mathbb{S}^2$. Buktikan bahwa terdapat basis ortonormal $\mathcal{B} = \{u_1, u_2, u_3\}$ sehingga representasi matriks dari rotasi $r = \bar{r}_{G_1} \circ \bar{r}_{G_2}$ berbentuk blok rotasi murni:
  $$[r]_{\mathcal{B}} = \begin{bmatrix} \cos\theta & -\sin\theta & 0 \\ \sin\theta & \cos\theta & 0 \\ 0 & 0 & 1 \end{bmatrix} \in SO(3).$$

</div>
