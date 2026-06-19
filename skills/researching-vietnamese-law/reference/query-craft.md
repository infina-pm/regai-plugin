# Writing good search queries

`search` and `deep-research` match on the terms you give them, so result quality
depends on the terms you feed them. Load this when hits are thin, noisy, or off-topic.

1. **Pull the legal nouns/verbs; drop filler.** From "Công ty tôi muốn phát hành thêm
   cổ phiếu cho cổ đông hiện hữu thì cần điều kiện gì?" extract
   `phát hành cổ phiếu cổ đông hiện hữu điều kiện`.

2. **Recognize a số hiệu and search it directly.** Patterns like `155/2020/NĐ-CP` or
   `54/2019/QH14` pull the exact document.

3. **Use canonical legal terms, not colloquial ones:** prefer `chào bán chứng khoán
   ra công chúng` over "bán cổ phiếu", `vốn điều lệ` over "tiền vốn", `người nội bộ`
   over "sếp công ty".

4. **Keep correct diacritics** — they improve precision.

5. **Broaden, then narrow.** Start with 2–3 core terms. Too few hits → drop the most
   specific term. Too noisy → add a distinguishing term (e.g. `nghị định`, or the
   domain like `chứng khoán`).

6. **Follow leads with the graph, not more keyword guessing.** Once you have one
   relevant document, use `list-related` and the `relations` from `get-document` to
   reach its implementing decrees/circulars instead of re-searching.
