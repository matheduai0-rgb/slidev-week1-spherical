# Geometri Lingkaran Besar & Garis Spherical $G_{p_1, p_2}$ (Lee 2.1)

<div class="grid grid-cols-12 gap-4 items-center">

<div class="col-span-6">

<div class="def-box">

**Definisi (Lingkaran Besar vs. Lingkaran Kecil & Garis Spherical).**

- Irisan $\mathbb{S}^2 \cap P$ dengan bidang $P \subset \mathbb{R}^3$ yang **melalui titik asal $0$** disebut **lingkaran besar** (*great circle*). Jika $P$ tidak melalui $0$, irisannya disebut **lingkaran kecil** (*small circle*).
- Untuk dua titik berbeda $p_1, p_2 \in \mathbb{S}^2$, definisikan tempat kedudukan ekuidistan:
  $$G_{p_1, p_2} = \{ p \in \mathbb{S}^2 \mid d_{\mathbb{S}^2}(p, p_1) = d_{\mathbb{S}^2}(p, p_2) \}.$$
- Himpunan $G \subset \mathbb{S}^2$ disebut **garis spherical** jika $G = G_{p_1, p_2}$ untuk suatu $p_1 \neq p_2$.
- Tiga titik $p_1, p_2, p_3 \in \mathbb{S}^2$ disebut **segaris** (*collinear*) jika ketiganya termuat pada satu garis spherical $G$ yang sama.

</div>

<div v-click class="thm-box">

**Konsekuensi Fundamental pada $\mathbb{S}^2$:** Setiap lingkaran besar $G = \mathbb{S}^2 \cap P$ adalah garis spherical $G_{p_1, -p_1}$ dengan $\pm p_1 \in \mathbb{S}^2$ sebagai pasangan kutub normal bidang $P$.

</div>

</div>

<div class="col-span-6">

<JxgEquidistantSphere height="255px" :showSmallCircle="true" />

</div>

</div>

---

# Proposisi 2.3: Garis Spherical $G_{p_1, p_2}$ adalah Lingkaran Besar

<div class="grid grid-cols-12 gap-4 items-center">

<div class="col-span-7">

<div class="thm-box">

**Proposisi 2.3 (Lee — Bentuk Kanonik $H \Rightarrow C$).**

- **Hipotesis $H$:** $S = G_{p_1, p_2}$ adalah garis spherical dengan $p_1, p_2 \in \mathbb{S}^2$ titik-titik yang berbeda.
- **Kesimpulan $C$:** $S$ adalah suatu lingkaran besar pada $\mathbb{S}^2$ (dan akibatnya setiap dua garis spherical berbeda berpotongan di tepat dua titik antipodal $\{q, -q\}$).

</div>

<div v-click class="proof-box">

**Bukti Formal Deduktif (Aturan Logika 1 Greenberg):**

1. Misalkan $S = G_{p_1, p_2}$ dengan $p_1, p_2 \in \mathbb{S}^2$ dan $p_1 \neq p_2$ <span class="just">(Hipotesis $H$)</span>.
2. Untuk $p \in \mathbb{S}^2$, kondisi $d_{\mathbb{S}^2}(p, p_1) = d_{\mathbb{S}^2}(p, p_2)$ ekuivalen dengan $d(p, p_1) = d(p, p_2)$ <span class="just">(Lema 1)</span>.
3. Di $\mathbb{R}^3$, himpunan $P = \{p \in \mathbb{R}^3 \mid d(p, p_1) = d(p, p_2)\}$ adalah bidang $\{p \mid \langle p, p_1 - p_2 \rangle = 0\}$ karena $|p - p_1|^2 = |p - p_2|^2 \iff \langle p, p_1 \rangle = \langle p, p_2 \rangle \iff \langle p, p_1 - p_2 \rangle = 0$ <span class="just">(Sifat Hasil Kali Titik & $|p_1|=|p_2|=1$)</span>.
4. Karena $d(0, p_1) = 1 = d(0, p_2)$, titik asal $0$ termuat di dalam bidang $P$ <span class="just">(Definisi $\mathbb{S}^2$)</span>.
5. Maka $S = \{p \in \mathbb{S}^2 \mid d(p, p_1) = d(p, p_2)\} = \mathbb{S}^2 \cap P$ adalah irisan $\mathbb{S}^2$ dengan bidang melalui $0$, yakni sebuah lingkaran besar <span class="just">(Langkah 2–4 & Definisi Lingkaran Besar)</span>. <span class="qed">∎</span>

</div>

</div>

<div class="col-span-5">

<JxgEquidistantSphere height="255px" :showSmallCircle="false" />

</div>

</div>

---

# Lemma 2.4 & Teorema 2.5: Teorema Tiga Titik untuk $\mathbb{S}^2$

<div class="grid grid-cols-12 gap-4 items-center">

<div class="col-span-11">

<div class="thm-box">

**Lemma 2.4 (Pengawetan Ketaksegarisan — Lee).** Jika $\phi \in \operatorname{Iso}(\mathbb{S}^2)$ dan $p_1, p_2, p_3 \in \mathbb{S}^2$ tak segaris, maka $\phi(p_1), \phi(p_2), \phi(p_3)$ juga tak segaris.

*Bukti RAA:* Andaikan $\phi(p_i) \in G_{q_1, q_2}$ ($i=1,2,3$) <span class="just">(Hipotesis RAA)</span>. Karena $\phi$ mengawetkan jarak, $d_{\mathbb{S}^2}(\phi^{-1}(q_1), p_i) = d_{\mathbb{S}^2}(q_1, \phi(p_i)) = d_{\mathbb{S}^2}(q_2, \phi(p_i)) = d_{\mathbb{S}^2}(\phi^{-1}(q_2), p_i)$, sehingga $p_1, p_2, p_3 \in G_{\phi^{-1}(q_1), \phi^{-1}(q_2)}$ (segaris) — kontradiksi! <span class="qed">∎</span>

</div>

<div v-click class="thm-box">

**Teorema 2.5 (Teorema Tiga Titik untuk $\mathbb{S}^2$ / Three Points Theorem).**

- **Hipotesis $H$:** $\phi, \psi \in \operatorname{Iso}(\mathbb{S}^2)$ dan terdapat 3 titik tak segaris $p_1, p_2, p_3 \in \mathbb{S}^2$ sehingga $\phi(p_i) = \psi(p_i)$ ($i=1,2,3$).
- **Kesimpulan $C$:** $\phi = \psi$ pada seluruh $\mathbb{S}^2$.

</div>

<div v-click class="proof-box">

**Bukti Formal RAA (Aturan Logika 2 Greenberg):**

1. Berdasarkan Lemma 2.4, titik-titik bayangan $\phi(p_1), \phi(p_2), \phi(p_3)$ tak segaris <span class="just">(Hipotesis $H$ & Lemma 2.4)</span>.
2. **Hipotesis RAA:** Andaikan $\phi \neq \psi$, maka terdapat $p \in \mathbb{S}^2$ sehingga $\phi(p) \neq \psi(p)$ <span class="just">(Aturan Logika 2 & 6)</span>.
3. Karena $\phi(p) \neq \psi(p)$, garis spherical $G = G_{\phi(p), \psi(p)}$ terdefinisi dengan baik <span class="just">(Langkah 2 & Definisi Garis Spherical)</span>.
4. Untuk setiap $i \in \{1, 2, 3\}$, berlaku $d_{\mathbb{S}^2}(\phi(p), \phi(p_i)) = d_{\mathbb{S}^2}(p, p_i) = d_{\mathbb{S}^2}(\psi(p), \psi(p_i)) = d_{\mathbb{S}^2}(\psi(p), \phi(p_i))$ <span class="just">(Definisi Isometri & Hipotesis $\phi(p_i)=\psi(p_i)$)</span>.
5. Maka $\phi(p_1), \phi(p_2), \phi(p_3) \in G_{\phi(p), \psi(p)}$ (segaris) — kontradiksi dengan Langkah 1! Jadi $\phi = \psi$ <span class="just">(Kesimpulan RAA)</span>. <span class="qed">∎</span>

</div>

</div>



</div>

---

# Diskusi Sokratik & Latihan Kuliah 1 (Lee Subbab 2.1)

<div class="socratic-box">

**Diskusi Sokratik:** Apa keuntungan mendefinisikan garis spherical sebagai **tempat kedudukan ekuidistan** $G_{p_1, p_2}$? Bagaimana rumus tali busur $d(p_1, p_2) = 2\sin(d_{\mathbb{S}^2}/2)$ mengizinkan kita mengimpor bidang pembagi tegak lurus Euklides di $\mathbb{R}^3$ langsung ke $\mathbb{S}^2$ dan membuktikan Teorema Tiga Titik (Teorema 2.5) secara ringkas?

</div>

<div v-click class="latihan-box mt-4">

- **Latihan 2.1 (Lee):** Untuk $p_1 = \tfrac{1}{\sqrt{2}}(1, 1, 0)$ dan $p_2 = \tfrac{1}{\sqrt{2}}(1, 0, 1)$ pada $\mathbb{S}^2$, hitunglah jarak Euklides $d(p_1, p_2)$ dan jarak spherical $d_{\mathbb{S}^2}(p_1, p_2)$ serta tentukan persamaan bidang yang memuat $G_{p_1, p_2}$.
- **Latihan 2.2 (Lee):** Buktikan bahwa tidak ada pemetaan $f: \mathbb{R}^2 \to \mathbb{S}^2$ yang mengawetkan jarak secara global ($d(p_1, p_2) = d_{\mathbb{S}^2}(f(p_1), f(p_2))$ untuk semua $p_1, p_2 \in \mathbb{R}^2$). *(Petunjuk: Di $\mathbb{R}^2$ jarak tak terbatas, sedangkan pada $\mathbb{S}^2$ berlaku $d_{\mathbb{S}^2} \le \pi$.)*
- **Latihan 2.3 (Lee):** Lingkaran pada $\mathbb{S}^2$ dengan pusat spherical $p$ dan jari-jari spherical $r \in (0, \pi)$ didefinisikan oleh $\{q \in \mathbb{S}^2 \mid d_{\mathbb{S}^2}(p, q) = r\}$. Buktikan bahwa kelilingnya adalah $2\pi\sin r < 2\pi r$.

</div>
