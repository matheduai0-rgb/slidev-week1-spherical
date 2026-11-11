# MA3042 Geometri — Minggu 1 (Kuliah 1 & 2)

> **Geometri Bola $\mathbb{S}^2$ dan Struktur Grup Isometri $\operatorname{Iso}(\mathbb{S}^2)$**  
> Slide kuliah interaktif berbasis [Slidev](https://sli.dev/) dan [JSXGraph](https://jsxgraph.uni-bayreuth.de/).

🌐 **Akses Slide Online (Live Web):**  
👉 **[https://matheduai0-rgb.github.io/slidev-week1-spherical/](https://matheduai0-rgb.github.io/slidev-week1-spherical/)**

---

## 📚 Cakupan Materi

1. **Kuliah 1: Metrik Spherical & Garis Lingkaran Besar (Lee Subbab 2.1)**
   - Bola Satuan $\mathbb{S}^2 \subset \mathbb{R}^3$, metrik $d_{\mathbb{S}^2}(p, q) = \arccos(\langle p, q \rangle)$, dan hubungan tali busur Euclidean $d(p, q) = 2\sin(d_{\mathbb{S}^2}/2)$.
   - Isometri pada $\mathbb{S}^2$ sebagai restriksi isometri $\mathbb{R}^3$ yang menetapkan titik asal $O$ ($f(0) = 0$).
   - Karakterisasi garis spherical sebagai irisan bidang melalui titik asal dengan $\mathbb{S}^2$ (lingkaran besar).
   - Garis ekuidistan $G_{p_1, p_2}$ dan Teorema Kekakuan Tiga Titik.

2. **Kuliah 2: Refleksi Spherical & Teorema Tiga Refleksi (Lee Subbab 2.2)**
   - Refleksi spherical $\bar{r}_G(p) = p - 2\langle p, u \rangle u$ terhadap cermin lingkaran besar $G = u^\perp \cap \mathbb{S}^2$.
   - Teorema Tiga Refleksi untuk $\mathbb{S}^2$: setiap isometri $\phi \in \operatorname{Iso}(\mathbb{S}^2)$ merupakan komposisi paling banyak 3 refleksi spherical.
   - Sifat ketunggalan dan sifat komutasi refleksi tegak lurus.

3. **Modul Pengayaan: Formulasi Analitik Basis Ortonormal & Rotasi (Ryan Bab 1 & 3)**
   - Basis ortonormal terorientasi $\mathcal{B} = \{P, Q, \eta\}$ dan matriks Householder refleksi:
     $$[\bar{r}_P]_\mathcal{B} = \begin{pmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & -1 \end{pmatrix}$$
   - Representasi rotasi meridian $\bar{r}_\alpha \bar{r}_\beta = \operatorname{rot}(2(a-b))$ terhadap kutub $\pm\eta$.
   - Teorema Euler untuk $\mathbb{S}^2$ dan Isomorfisma Kanonik $O(3) \cong \operatorname{Iso}(\mathbb{S}^2)$.

---

## 🚀 Menjalankan Secara Lokal

```bash
# Clone repository
git clone https://github.com/matheduai0-rgb/slidev-week1-spherical.git
cd slidev-week1-spherical

# Install dependensi
npm install

# Jalankan server pengembangan
npm run dev
```

Buka peramban di `http://localhost:3030`.

## 🛠️ Build untuk Produksi

```bash
npm run build
```
Hasil build statis tersimpan di direktori `dist/`.
