# Kuliah 2: Refleksi Spherical $\bar{r}_G$ & Sifat Pertukaran (Lee 2.2)

<div class="grid grid-cols-12 gap-4 items-center">

<div class="col-span-7">

<div class="def-box">

**Definisi (Refleksi Spherical $\bar{r}_G$).** Misalkan $G = \mathbb{S}^2 \cap P$ adalah lingkaran besar dengan bidang cermin $P \subset \mathbb{R}^3$ melalui $0$ bervektor normal satuan $u \in \mathbb{S}^2$. Refleksi Euklides terhadap bidang $P$,

$$\bar{r}_P(x) = x - 2\langle x, u \rangle u,$$

mengawetkan $(0,0,0)$. Berdasarkan Proposisi 2.2, restriksinya pada $\mathbb{S}^2$ mendefinisikan **refleksi spherical** $\bar{r}_G: \mathbb{S}^2 \to \mathbb{S}^2$.

</div>

<div v-click class="thm-box">

**Tiga Sifat Fundamental Refleksi Spherical $\bar{r}_G$:**

1. **Titik Tetap:** $\operatorname{Fix}(\bar{r}_G) = G$, yakni $\bar{r}_G(p) = p \iff p \in G$.
2. **Involusi:** $\bar{r}_G \circ \bar{r}_G = \operatorname{id}_{\mathbb{S}^2}$, sehingga $(\bar{r}_G)^{-1} = \bar{r}_G$.
3. **Sifat Pertukaran Pembagi Tegak Lurus:** Jika $G = G_{p_1, p_2}$ untuk $p_1 \neq p_2$, maka refleksi $\bar{r}_G$ mempertukarkan $p_1$ dan $p_2$:
   $$\bar{r}_{G_{p_1, p_2}}(p_1) = p_2 \quad \text{dan} \quad \bar{r}_{G_{p_1, p_2}}(p_2) = p_1.$$
   *(Bukti: Normal satuan bidang $P$ pembagi tegak lurus $p_1 p_2$ adalah $u = \frac{p_1 - p_2}{|p_1 - p_2|}$; karena $|p_1|=|p_2|=1$, berlaku $2\langle p_1, p_1 - p_2 \rangle = |p_1 - p_2|^2$, sehingga $\bar{r}_P(p_1) = p_1 - (p_1 - p_2) = p_2$.)*

</div>

</div>

<div class="col-span-5">

<JxgSphericalReflection height="255px" />

</div>

</div>

---

# Teorema 2.7: Teorema Tiga Refleksi untuk $\mathbb{S}^2$ (Kasus 1 & 2)

<div class="grid grid-cols-12 gap-4 items-center">

<div class="col-span-10">

<div class="thm-box">

**Teorema 2.7 (Three Reflections Theorem for $\mathbb{S}^2$ — Nam-Hoon Lee).**

- **Hipotesis $H$:** $\phi: \mathbb{S}^2 \to \mathbb{S}^2$ adalah sebarang isometri pada bola satuan $\mathbb{S}^2$.
- **Kesimpulan $C$:** $\phi$ adalah komposisi dari **paling banyak tiga** refleksi pada lingkaran-lingkaran besar.

</div>

<div v-click class="proof-box">

**Bukti Formal Berdasarkan 4 Kasus (Pilih 3 titik tak segaris $p_1, p_2, p_3 \in \mathbb{S}^2$):**

- **Kasus 1 (Ketiga titik tetap: $\phi(p_i) = p_i$ untuk $i=1,2,3$):** Berdasarkan Teorema 2.5 (dengan $\psi = \operatorname{id}_{\mathbb{S}^2}$), diperoleh $\phi = \operatorname{id}_{\mathbb{S}^2}$ (**$0$ refleksi**) <span class="just">(Teorema 2.5)</span>.
- **Kasus 2 (Tepat dua titik tetap: $\phi(p_1) = p_1, \phi(p_2) = p_2$, namun $\phi(p_3) \neq p_3$):**
  1. Bentuk garis spherical $G = G_{p_3, \phi(p_3)}$ <span class="just">(Definisi Garis Spherical karena $\phi(p_3) \neq p_3$)</span>.
  2. Untuk $i \in \{1, 2\}$, $d_{\mathbb{S}^2}(p_3, p_i) = d_{\mathbb{S}^2}(\phi(p_3), \phi(p_i)) = d_{\mathbb{S}^2}(\phi(p_3), p_i) \implies p_1, p_2 \in G$ <span class="just">(Definisi Isometri)</span>.
  3. Komposisi $\psi = \bar{r}_G \circ \phi$ memfiksasi $p_1, p_2 \in G$ dan $\psi(p_3) = \bar{r}_G(\phi(p_3)) = p_3$ <span class="just">(Sifat Pertukaran $\bar{r}_G$)</span>.
  4. Maka $\bar{r}_G \circ \phi = \operatorname{id}_{\mathbb{S}^2} \implies \phi = \bar{r}_G$ (**$1$ refleksi**) <span class="just">(Teorema 2.5 & Involusi)</span>.

</div>

</div>



</div>

---

# Teorema 2.7: Kasus 3 & Kasus 4 (Reduksi Induktif)

<div class="grid grid-cols-12 gap-4 items-center">

<div class="col-span-10">

<div class="proof-box">

**Lanjutan Bukti Formal Teorema 2.7 — Kasus 3 ($\phi(p_1) = p_1$, namun $\phi(p_2) \neq p_2, \phi(p_3) \neq p_3$):**

1. Karena $d_{\mathbb{S}^2}(p_2, p_1) = d_{\mathbb{S}^2}(\phi(p_2), \phi(p_1)) = d_{\mathbb{S}^2}(\phi(p_2), p_1)$, titik $p_1$ terletak pada lingkaran besar $H = G_{p_2, \phi(p_2)}$ <span class="just">(Definisi Isometri & Garis Spherical)</span>.
2. Definisikan $\phi' = \bar{r}_H \circ \phi$. Maka $\phi'(p_1) = \bar{r}_H(p_1) = p_1$ dan $\phi'(p_2) = \bar{r}_H(\phi(p_2)) = p_2$ <span class="just">(Titik Tetap & Sifat Pertukaran)</span>.
3. Karena $\phi'$ memfiksasi sedikitnya dua titik ($p_1, p_2$), tereduksi ke Kasus 1 atau 2: $\phi' = \operatorname{id}_{\mathbb{S}^2}$ atau $\phi' = \bar{r}_G$, sehingga $\phi = \bar{r}_H$ atau $\phi = \bar{r}_H \circ \bar{r}_G$ (**$1$ atau $2$ refleksi**) <span class="just">(Kasus 1 & 2)</span>.

</div>

<div v-click class="proof-box">

**Kasus 4 (Tidak ada titik tetap: $\phi(p_i) \neq p_i$ untuk $i=1,2,3$):**

1. Ambil $N = G_{p_1, \phi(p_1)}$ dan definisikan $\phi'' = \bar{r}_N \circ \phi$ <span class="just">(Definisi Garis Spherical)</span>.
2. Maka $\phi''(p_1) = \bar{r}_N(\phi(p_1)) = p_1$, sehingga $\phi''$ memfiksasi $p_1$ dan tereduksi ke Kasus 1, 2, atau 3 <span class="just">(Sifat Pertukaran)</span>.
3. Oleh karena itu, $\phi = \bar{r}_N \circ \phi''$ adalah komposisi dari **paling banyak $3$ refleksi** <span class="just">(Aturan Logika 11 — Bukti Berdasarkan Kasus)</span>. <span class="qed">∎</span>

</div>

</div>



</div>
