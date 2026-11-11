# Teorema 2.8 & 2.10: Rotasi Spherical Euler & Dikotomi Orientasi

<div class="grid grid-cols-12 gap-4 items-center">

<div class="col-span-7">

<div class="thm-box">

**Teorema 3 refleksi.** Jika $\alpha,\beta,\gamma$ semuanya melewati titik $\eta$, maka terdapat garis $\delta$ yang melewati $\eta$ dan memenuhi $\bar{r}_{\alpha}\circ\bar{r}_{\beta}=\bar{r}_{\gamma}\circ\bar{r}_{\delta}.$


</div>

<div v-click class="thm-box">

**Teorema 2.8 (Teorema Rotasi Euler — Lee).** Komposisi dari dua rotasi pada $\mathbb{S}^2$ adalah suatu rotasi.

*Bukti:* Misalkan $r_1 = \bar{r}_{G_1} \circ \bar{r}_{G_2}$ dan $r_2 = \bar{r}_{G_3} \circ \bar{r}_{G_4}$ dengan $G_2 \cap G_3 = \{p, -p\}$. Berdasarkan Teorema 7 (eksistensi garis keempat melalui $\pm p$), terdapat lingkaran besar $G_2'$ melalui $p, -p$ sehingga $\bar{r}_{G_1} \circ \bar{r}_{G_2} = \bar{r}_{G_2'} \circ \bar{r}_{G_3}$. Maka:

$$r_1 \circ r_2 = \bar{r}_{G_2'} \circ (\bar{r}_{G_3} \circ \bar{r}_{G_3}) \circ \bar{r}_{G_4} = \bar{r}_{G_2'} \circ \bar{r}_{G_4} \quad \text{(suatu rotasi!)}. \quad \blacksquare$$

</div>

<div v-click class="thm-box">

**Lemma 2.9 & Teorema 2.10 (Dikotomi Orientasi — Lee).** Definisikan $\operatorname{Iso}^+(\mathbb{S}^2)$ (komposisi **genap** refleksi / rotasi) dan $\operatorname{Iso}^-(\mathbb{S}^2)$ (komposisi **ganjil** refleksi). Berdasarkan Teorema 2.7 dan Teorema 2.8, setiap elemen $\operatorname{Iso}^+(\mathbb{S}^2)$ adalah komposisi $0$ atau $2$ refleksi (memiliki titik tetap antipodal $\{p, -p\}$), dan berlaku partisi saling lepas:

$$\operatorname{Iso}(\mathbb{S}^2) = \operatorname{Iso}^+(\mathbb{S}^2) \sqcup \operatorname{Iso}^-(\mathbb{S}^2), \quad \operatorname{Iso}^+(\mathbb{S}^2) \cap \operatorname{Iso}^-(\mathbb{S}^2) = \emptyset.$$

</div>

</div>

<div class="col-span-5">

<JxgRyanMeridianRotation :showFourthLine="true" height="255px" />

</div>

</div>

---

# Definisi 2.15 & Proposisi 2.16–2.17: Grup Ortogonal $O(3)$ (Lee 2.4)

<div class="grid grid-cols-12 gap-4 items-center">

<div class="col-span-12">

<div class="def-box">

**Definisi 2.15 (Transformasi Ortogonal — Lee).** Suatu isometri $\phi: \mathbb{R}^3 \to \mathbb{R}^3$ disebut **transformasi ortogonal** jika $\phi$ menetapkan titik asal: $\phi(0) = 0$. Himpunan seluruh transformasi ortogonal pada $\mathbb{R}^3$ dinotasikan dengan $O(3)$.

</div>

<div v-click class="thm-box">

**Proposisi 2.16 & 2.17 (Karakterisasi Hasil Kali Titik & Matriks — Lee).** Untuk setiap $\phi \in O(3)$ dan setiap $p, q \in \mathbb{R}^3$, berlaku:

1. **Pengawetan Hasil Kali Titik:** $\langle \phi(p), \phi(q) \rangle = \langle p, q \rangle$.

   *(Bukti: $|\phi(p) - \phi(q)|^2 = |p - q|^2$ dan $|\phi(p)| = d(\phi(0), \phi(p)) = |p|$, sehingga ekspansi kuadrat memberikan $-2\langle \phi(p), \phi(q) \rangle = -2\langle p, q \rangle$.)*
2. **Kelinearan & Basis Ortonormal:** $\phi$ memetakan basis standar ortonormal $\{e_1, e_2, e_3\}$ ke basis ortonormal $\{\phi(e_1), \phi(e_2), \phi(e_3)\}$ dan bersifat linier:
   $$\phi(x e_1 + y e_2 + z e_3) = x \phi(e_1) + y \phi(e_2) + z \phi(e_3).$$
3. **Representasi Matriks Ortogonal:** Dalam matriks kolom $A = [\phi(e_1) \ \phi(e_2) \ \phi(e_3)]$, kondisi ortonormalitas kolom ekuivalen dengan $A^T A = I_3$ (dengan $\det(A) = \pm 1$). <span class="qed">∎</span>

</div>

</div>



</div>

---

# Teorema 2.18: Isomorfisma Kanonik $O(3) \cong \operatorname{Iso}(\mathbb{S}^2)$

<div class="grid grid-cols-12 gap-4 items-center">

<div class="col-span-12">

<div class="thm-box">

**Teorema 2.18 (Isomorfisma Grup $\Psi: O(3) \stackrel{\sim}{\to} \operatorname{Iso}(\mathbb{S}^2)$ — Lee).** Pemetaan restriksi $\Psi: O(3) \to \operatorname{Iso}(\mathbb{S}^2)$ yang didefinisikan oleh $\Psi(\phi) = \phi|_{\mathbb{S}^2}$ adalah suatu **bijeksi** yang memenuhi:

$$\Psi(\phi_1 \circ \phi_2) = \Psi(\phi_1) \circ \Psi(\phi_2) \quad \text{dan} \quad \Psi(\phi^{-1}) = \Psi(\phi)^{-1}.$$

</div>

<div v-click class="proof-box">

**Bukti**

1. Untuk setiap $\phi \in \operatorname{Iso}(\mathbb{S}^2)$, definisikan **perluasan radial** $\tilde{\phi}: \mathbb{R}^3 \to \mathbb{R}^3$ dengan:
   $$\tilde{\phi}(p) = |p| \cdot \phi\!\left(\frac{p}{|p|}\right) \quad (p \neq 0) \quad \text{dan} \quad \tilde{\phi}(0) = 0 \quad \text{(Konstruksi Pemetaan Balik } \Phi\text{)}.$$
2. Untuk $p, q \in \mathbb{R}^3 \setminus \{0\}$, tulis $\hat{p} = \frac{p}{|p|}, \hat{q} = \frac{q}{|q|} \in \mathbb{S}^2$. Karena $\phi \in \operatorname{Iso}(\mathbb{S}^2)$, berlaku 
$$\langle \phi(\hat{p}), \phi(\hat{q}) \rangle = \cos d_{\mathbb{S}^2}(\phi(\hat{p}), \phi(\hat{q})) = \cos d_{\mathbb{S}^2}(\hat{p}, \hat{q}) = \langle \hat{p}, \hat{q} \rangle$$ 


3. Kalikan dengan $|p||q|$: diperoleh $\langle \tilde{\phi}(p), \tilde{\phi}(q) \rangle = \langle p, q \rangle \implies |\tilde{\phi}(p) - \tilde{\phi}(q)| = |p - q| \implies \tilde{\phi} \in O(3)$ <span class="just">(Langkah 1 & 2)</span>.
4. Verifikasi langsung menunjukkan $(\Phi \circ \Psi)(\phi) = \phi$ dan $(\Psi \circ \Phi)(\psi) = \psi$, sehingga $\Psi$ adalah isomorfisma grup bijektif <span class="just">(Definisi Isomorfisma)</span>. <span class="qed">∎</span>

</div>

<div v-click class="def-box">

**Akibat 2.19 & 2.20 (Lee):** Setiap transformasi di $O(3)$ adalah komposisi $\le 3$ refleksi bidang melalui $0$; setiap isometri di $\mathbb{R}^3$ adalah komposisi $\le 4$ refleksi bidang.

</div>

</div>



</div>

---

# Diskusi Sokratik & Latihan Kuliah 2 (Lee 2.2 & 2.4)

<div class="socratic-box">

**Pertanyaan Sokratik:** Mengapa di permukaan bola $\mathbb{S}^2$ tidak terdapat "translasi" non-trivial yang terpisah dari rotasi? Bagaimana fakta bahwa setiap dua lingkaran besar berpotongan di sepasang titik antipodal meleburkan translasi dan rotasi menjadi satu kelas tunggal?

</div>

<div v-click class="exercise-box">

**Latihan Terstruktur Kuliah 2 (Lee Bab 2):**

- **Latihan 2.4 (Lee):** Tentukan apakah peta $(x, y, z) \mapsto (-x, -y, z)$ dan $(x, y, z) \mapsto (-x, y, z)$ termasuk di dalam $\operatorname{Iso}^+(\mathbb{S}^2)$ atau $\operatorname{Iso}^-(\mathbb{S}^2)$.
- **Latihan 2.5 (Lee — Peta Antipodal $\hat{a}(p) = -p$):**
  - (a) Buktikan bahwa $\hat{a} \in \operatorname{Iso}^-(\mathbb{S}^2)$ adalah komposisi $3$ refleksi pada lingkaran-lingkaran besar koordinat dan bebas titik tetap ($\operatorname{Fix}(\hat{a}) = \emptyset$).
  - (b) Buktikan bahwa setiap $\phi \in \operatorname{Iso}^-(\mathbb{S}^2)$ dapat ditulis sebagai $\phi = \hat{a} \circ r$ untuk suatu rotasi $r \in \operatorname{Iso}^+(\mathbb{S}^2)$.
- **Latihan 2.6 (Lee):** Untuk lingkaran besar $G$ dan $\phi \in \operatorname{Iso}(\mathbb{S}^2)$, buktikan rumus konjugasi refleksi:
  $$\phi \circ \bar{r}_G \circ \phi^{-1} = \bar{r}_{\phi(G)}.$$
- **Latihan 2.8 (Lee):** Klasifikasikan seluruh involusi pada $\mathbb{S}^2$, yakni semua $\phi \in \operatorname{Iso}(\mathbb{S}^2)$ yang memenuhi $\phi^2 = \operatorname{id}_{\mathbb{S}^2}$.

</div>
