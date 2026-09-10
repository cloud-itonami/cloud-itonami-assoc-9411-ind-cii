(ns association.facts
  "Industry rule/history catalog for the Confederation of Indian
  Industry (CII) -- a 48th industry-association-level source (see
  cloud-itonami-assoc-9411-sau-fsc, -9411-aut-wko, -9411-irl-ibec,
  -9411-nzl-businessnz, -9411-cze-spcr for the first five) per
  ADR-2607141700 (cloud-itonami-compliance-fact-federation). The
  SIXTH entry aligned to ISIC 9411 (activities of business,
  employers, and professional membership organizations). Fills
  India's previously-open association-axis gap -- India now has
  real, individually verified facts across ALL THREE axes (country:
  cloud-itonami-iso3166-ind statute.facts; municipality:
  cloud-itonami-municipality-ind-new-delhi; association: this
  entry).

  Both entries directly WebFetch-verified against cii.in's own
  'History' page (https://www.cii.in/about_us_History.aspx?gid=A),
  which quotes verbatim: 'The journey began in 1895 when 5
  engineering firms, all members of the Bengal Chamber of Commerce
  and Industry, joined hands to form the Engineering and Iron Trades
  Association (EITA)' and 'With effect from 1st January 1992, in
  keeping with the government's decision to opt for the
  liberalisation of the Indian economy, the name of CEI was changed
  to Confederation of Indian Industry (CII)'. The 1895 date is
  independently corroborated by Wikidata Q842084's own 'inception'
  statement (1895). No personal names of office-holders are
  persisted here.

  An association not in `catalog` has NO spec-basis, full stop; never
  fabricate one.")

(def catalog
  "association-slug -> vector of association-rule entries."
  {"cii"
   [{:association-rule/id "cii.founding-1895-eita"
     :association-rule/title "Engineering and Iron Trades Association (EITA), CII's earliest predecessor, founded by 5 engineering firms in the Bengal Chamber of Commerce and Industry (cii.in official History page, corroborated by Wikidata Q842084 inception statement)"
     :association-rule/association "cii"
     :association-rule/isic "9411"
     :association-rule/country "IND"
     :association-rule/kind :governance-program
     :association-rule/url "https://www.cii.in/about_us_History.aspx?gid=A"
     :association-rule/url-provenance :official-cii-in
     :association-rule/established-date "1895"
     :association-rule/retrieved-at "2026-07-17"
     :association-rule/topic #{:governance}}
    {:association-rule/id "cii.renaming-1992-cii"
     :association-rule/title "Confederation of Engineering Industry renamed to Confederation of Indian Industry (CII), effective 1 January 1992, amid economic liberalisation (cii.in official History page)"
     :association-rule/association "cii"
     :association-rule/isic "9411"
     :association-rule/country "IND"
     :association-rule/kind :governance-program
     :association-rule/url "https://www.cii.in/about_us_History.aspx?gid=A"
     :association-rule/url-provenance :official-cii-in
     :association-rule/established-date "1992-01-01"
     :association-rule/retrieved-at "2026-07-17"
     :association-rule/topic #{:governance}}]})

(defn spec-basis [association] (get catalog association))

(defn coverage
  ([] (coverage (keys catalog)))
  ([associations]
   (let [have (filter catalog associations)
         missing (remove catalog associations)]
     {:requested (count associations)
      :covered (count have)
      :covered-associations (vec (sort have))
      :missing-associations (vec (sort missing))
      :note (str "cloud-itonami-assoc-9411-ind-cii Wave 0 (ADR-2607141700): "
                 (count (get catalog "cii")) " CII entries seeded "
                 "with cii.in official History page + Wikidata Q842084 corroboration. "
                 "Extend `association.facts/catalog`, never fabricate an id/url.")})))

(defn by-topic [association topic]
  (filterv #(contains? (:association-rule/topic %) topic) (spec-basis association)))
