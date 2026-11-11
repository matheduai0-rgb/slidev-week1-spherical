# Teorema 1: Kebebasan Linier & Koordinat Fourier pada Basis Ortonormal

<div class="grid grid-cols-12 gap-4 items-center">

<div class="col-span-10">

<div class="thm-box">

**Teorema 1 (Karakterisasi Basis Ortonormal — Bentuk Kanonik $H \implies C$).**

- **Hipotesis $H$:** $\mathcal{B} = \{u_1, u_2, \dots, u_n\} \subset \mathbb{R}^n$ memenuhi $\langle u_i, u_j \rangle = \delta_{ij}$.
- **Kesimpulan $C$:** (a) $\mathcal{B}$ bebas linier dan merupakan basis bagi $\mathbb{R}^n$; (b) untuk setiap $x \in \mathbb{R}^n$, berlaku representasi koordinat Fourier unik:
  $$x = \sum_{i=1}^n \langle x, u_i \rangle u_i = \langle x, u_1 \rangle u_1 + \dots + \langle x, u_n \rangle u_n, \quad [x]_{\mathcal{B}} = \begin{bmatrix} \langle x, u_1 \rangle \\ \vdots \\ \langle x, u_n \rangle \end{bmatrix}.$$

</div>

<div v-click class="proof-box">

**Bukti:**

1. Tinjau kombinasi linier nol $c_1 u_1 + \dots + c_n u_n = 0$ <span class="just">(Hipotesis Kebebasan Linier)</span>.
2. Untuk setiap indeks $k \in \{1, \dots, n\}$, ambil hasil kali titik kedua ruas dengan $u_k$:

   $$0 = \left\langle \sum_{i=1}^n c_i u_i, u_k \right\rangle = \sum_{i=1}^n c_i \langle u_i, u_k \rangle = \sum_{i=1}^n c_i \delta_{ik} = c_k \quad \text{(Bilinearitas & Hipotesis } H\text{)}.$$

3. Karena $c_1 = \dots = c_n = 0$, $\mathcal{B}$ bebas linier; karena $|\mathcal{B}| = n = \dim(\mathbb{R}^n)$, $\mathcal{B}$ adalah basis <span class="just">(Dimensi Aljabar Linier)</span>.
4. Sebarang $x \in \mathbb{R}^n$ ditulis unik $x = \sum_{i=1}^n \alpha_i u_i$; hasil kali titik dengan $u_k$ menghasilkan $\langle x, u_k \rangle = \alpha_k$ <span class="just">(Langkah 2)</span>. <span class="qed">∎</span>

</div>

</div>


</div>

---

# Proposisi 2: Identitas Parseval & Basis Teradaptasi pada $\mathbb{S}^2$

<div class="grid grid-cols-12 gap-4 items-center">

<div class="col-span-8">

<div class="thm-box">

**Proposisi 2 (Pengawetan Metrik dalam Koordinat Basis Ortonormal $\mathcal{B}$).** Jika $\mathcal{B} = \{u_1, u_2, u_3\}$ adalah basis ortonormal bagi $\mathbb{R}^3$, maka untuk setiap $x, y \in \mathbb{R}^3$:

$$\langle x, y \rangle = [x]_{\mathcal{B}}^T [y]_{\mathcal{B}} = \sum_{i=1}^3 \langle x, u_i \rangle \langle y, u_i \rangle \quad \text{dan} \quad |x|^2 = \sum_{i=1}^3 \langle x, u_i \rangle^2.$$

*Artinya: Dalam basis ortonormal, aljabar hasil kali titik dan norma panjang vektor identik persis dengan koordinat standar tanpa bobot metrik tambahan!*

</div>

<div v-click class="def-box">

**Konstruksi Basis Ortonormal Teradaptasi pada Lingkaran Besar $G \subset \mathbb{S}^2$:** Diberikan lingkaran besar $G = \mathbb{S}^2 \cap P$, dengan $P = \{x \in \mathbb{R}^3 \mid \langle x, u_3 \rangle = 0\}$ dan $|u_3| = 1$:

1. **Kutub Normal:** Pilih $u_3$ sebagai vektor normal satuan dari bidang $P$ (titik kutub di $\mathbb{S}^2$).
2. **Vektor Tangensial Pertama:** Pilih sebarang titik $u_1 \in G \subset \mathbb{S}^2$. Karena $u_1 \in P$, maka $\langle u_1, u_3 \rangle = 0$ dan $|u_1| = 1$.
3. **Vektor Tangensial Kedua:** Definisikan $u_2 = u_3 \times u_1$. Berdasarkan sifat perkalian silang, $u_2 \in P$, $|u_2| = |u_3||u_1|\sin(\pi/2) = 1$, dan $u_2 \perp u_1, u_2 \perp u_3$.
4. **Hasil:** $\mathcal{B} = \{u_1, u_2, u_3\}$ membentuk basis ortonormal yang memenuhi  syarat aturan tangan kanan yaitu $u_1\times u_2 = u_3$  dengan $\operatorname{span}\{u_1, u_2\} = P=\{\langle x, u_3 \rangle = 0\}$ dan $G = \mathbb{S}^2 \cap \operatorname{span}\{u_1, u_2\}$.

</div>

</div>

<div class="col-span-4">

<JxgOrthonormalBasis height="255px" />

</div>


</div>

---

# Teorema 3: Matriks Perubahan Basis antar Basis Ortonormal

<div class="grid grid-cols-12 gap-4 items-center">

<div class="col-span-11">

<div class="def-box">

Misalkan $\mathcal{E} = \{e_1, e_2, e_3\}$ adalah basis standar $\mathbb{R}^3$ dan $\mathcal{B} = \{u_1, u_2, u_3\}$ adalah basis sebarang. Matriks kolom $P = [u_1 \ u_2 \ u_3] \in \mathbb{R}^{3 \times 3}$ memenuhi:

$$x = [x]_1 u_1 + [x]_2 u_2 + [x]_3 u_3 = [u_1 \ u_2 \ u_3] [x]_{\mathcal{B}} \implies [x]_{\mathcal{E}} = P [x]_{\mathcal{B}}.$$

</div>

<div v-click class="thm-box">

**Teorema 3 (Karakterisasi Matriks Transisi Ortogonal — Bentuk Kanonik $H \implies C$).**

- **Hipotesis $H$:** $P = [u_1 \ u_2 \ u_3]$ adalah matriks transisi dari basis $\mathcal{B}$ ke basis standar $\mathcal{E}$.
- **Kesimpulan $C$:** $\mathcal{B}$ adalah basis ortonormal jika dan hanya jika $P \in O(3)$, yakni:
  $$P^T P = I_3 \iff P^{-1} = P^T = \begin{bmatrix} u_1^T \\ u_2^T \\ u_3^T \end{bmatrix} \quad \text{dan} \quad [x]_{\mathcal{B}} = P^T [x]_{\mathcal{E}}.$$

</div>

<div v-click class="proof-box">

**Bukti Formal Deduktif (Aturan Logika 1 Greenberg):**

1. Hitung perkalian blok matriks: $(P^T P)_{ij} = u_i^T u_j = \langle u_i, u_j \rangle$ <span class="just">(Aljabar Matriks Blok & Definisi Hasil Kali Titik)</span>.
2. Karena $\mathcal{B}$ ortonormal, $\langle u_i, u_j \rangle = \delta_{ij} \iff (P^T P)_{ij} = (I_3)_{ij} \iff P^T P = I_3$ <span class="just">(Definisi Basis Ortonormal)</span>.
3. Oleh karena itu, invers matriks diperoleh langsung tanpa eliminasi Gauss: $P^{-1} = P^T$ <span class="just">(Definisi Grup Ortogonal $O(3)$)</span>. <span class="qed">∎</span>

</div>

</div>


</div>

---

# Proposisi 4: Rumus Perubahan Basis Operator Linier pada $\mathbb{R}^3$

<div class="grid grid-cols-12 gap-4 items-center">

<div class="col-span-12">

<div class="thm-box">

**Proposisi 4 (Matriks Representasi Operator Linier $T: \mathbb{R}^3 \to \mathbb{R}^3$).** Misalkan $T: \mathbb{R}^3 \to \mathbb{R}^3$ adalah pemetaan linier, $\mathcal{E}$ basis standar, dan $\mathcal{B} = \{u_1, u_2, u_3\}$ basis ortonormal dengan matriks transisi $P = [u_1 \ u_2 \ u_3] \in O(3)$. Matriks representasi $[T]_{\mathcal{B}}$, yang didefinisikan oleh $[T(x)]_{\mathcal{B}} = [T]_{\mathcal{B}} [x]_{\mathcal{B}}$, memenuhi transformasi keserupaan ortogonal:

$$[T]_{\mathcal{B}} = P^{-1} [T]_{\mathcal{E}} P = P^T [T]_{\mathcal{E}} P \quad \Longleftrightarrow \quad [T]_{\mathcal{E}} = P [T]_{\mathcal{B}} P^T.$$

</div>

<div v-click class="proof-box">

**Diagram Komutatif & Bukti Aljabar:**

$$
\begin{array}{ccc}
[x]_{\mathcal{B}} & \xrightarrow{\quad [T]_{\mathcal{B}} \quad} & [T(x)]_{\mathcal{B}} \\
\Big\downarrow{\scriptstyle P} & & \Big\downarrow{\scriptstyle P} \\
[x]_{\mathcal{E}} & \xrightarrow[\quad [T]_{\mathcal{E}} \quad]{} & [T(x)]_{\mathcal{E}}
\end{array}
$$

1. Substitusikan $[x]_{\mathcal{E}} = P[x]_{\mathcal{B}}$ dan $[T(x)]_{\mathcal{E}} = P[T(x)]_{\mathcal{B}}$ ke persamaan $[T(x)]_{\mathcal{E}} = [T]_{\mathcal{E}}[x]_{\mathcal{E}}$ <span class="just">(Teorema 3)</span>.
2. Diperoleh $P[T(x)]_{\mathcal{B}} = [T]_{\mathcal{E}} P[x]_{\mathcal{B}} \implies [T(x)]_{\mathcal{B}} = (P^T [T]_{\mathcal{E}} P)[x]_{\mathcal{B}}$ karena $P^{-1} = P^T$ <span class="just">(Ketunggalan Matriks Operator)</span>. <span class="qed">∎</span>

</div>

</div>


</div>
