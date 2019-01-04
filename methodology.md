# Methodology

For what the construction of the word formation relationships is concerned, WFL uses a step-by-step morphotactic approach: derivational and compounding rules are modelled as directed one-to-many input-output relations between lexemes, and each word formation process  is treated individually. The lexeme resulting from a WFR is usually richer \(containing more morphemes\) than the input. with the exception of conversion, which only involves a change of PoS. Each output lexeme can only have one source, except in the case of compounds, where it is possible to have two \(or three\) input lexemes for one output lexeme.

##### **Building the Lexicon**

The word formation lexicon is built in two steps. First, word formation rules are detected. Then, they are applied to lexical data.

##### **Detecting Word Formation Rules**

Word formation rules \(WFRs\) are conceived according to the so-calledItem-and-Arrangement model, outlined by Hockett \(1954\), which considers word forms either as simple morphemes \(not derived word forms\) or as a concatenation of morphemes \(derived word forms\). The following conditions on bases and affixes do hold: \(1\) Baudoin’s assumption that both bases and affixes are lexical elements \(i.e. they are both morphemes\); \(2\) as a consequence, they exist in the lexicon \(Bloomfield’s “lexical morpheme” theory\); \(3\) they are dualistic, i.e. they have both form and meaning \(Bloomfield’s “sign-base” morpheme theory\). The first two conditions motivate the fact that in our word formation lexicon affixes are recorded with the same status of lexical bases; the third condition concerns the semantic properties of WFRs.

In Latin, WFRs fall into two main types: \(1\) derivation and \(2\) compounding. Derivation rules are further organised into two subcategories: \(a\) affixal, in its turn split into prefixal and suffixal, and \(b\) conversion, a derivation process that changes the PoS of the input word without affixation.

Compounding and conversion WFRs are automatically detected, by considering all the possible combinations of main PoS \(verbs, nouns, adjectives\), regardless of their actual instantiations in the lexical basis. For instance, there are four possible types of conversion WFRs involving verbs: V-To-N \(claudo &gt; clausa; 'to close' &gt; 'cell'\), V-To-A \(eligo &gt; elegans; 'to pick out' &gt; 'accustomed to select, tasteful'\), N-To-V \(magister &gt; magistro; 'master' &gt; 'to rule'\), A-To-V \(celer &gt; celero; 'quick' &gt; 'to quicken'\). Each compounding and conversion WFR type is further specified by the inflectional category of both input and output. For instance, A1-To-V1 is the conversion WFR from first class adjectives to first conjugation verbs.

Affixal WFRs are found both according to previous literature on Latin derivational morphology \(Jenks, 1911; Fruyt, 2011; Oniga, 1988\) and in semi-automatic fashion. The latter is performed by extracting from the list of lemmas of Lemlat the most frequent sequences of characters occurring on the left \(prefixes\) and on the right \(suffixes\) side of lemmas. The PoS for WFR input and output lemmas as well as their inflectional category are manually assigned. Further affixal WFRs are found by confrontation with data.

We recorded the rules in a table of a MySQL relational database where each WFR is classified by type and it is assigned the required PoS, inflectional category and gender for its input and output.

##### **Applying Word Formation Rules**

Each morphologically derived lemma is assigned a WFR. All those lemmas that share a common \(not derived\) ancestor belong to the same “word formation family”. For instance, lemmas _formatio_ 'formation', _formo_ 'to form' and formosus \(“beautiful”, lit. “finely formed”\) all belong to the word formation family whose ancestor is the lemma forma\(“form”\).

WFL uses a morphotactic approach. Each word formation process is treated individually, and the lexeme resulting from a WFR is usually richer \(containing more morphemes\) than the input. with the exception of conversion, which only involves a change of PoS. Each output lexeme can only have one source, except in the case of compounds, where it is possible to have two \(or three\) input lexemes for one output lexeme.

Lemmas and WFRs are paired by using a MySQL relational database whose main tables are the les archive of Lemlat, the list of its lemmas \(each assigned its PoS, inflectional category and, for nouns only, gender\) and the list of WFRs.

A number of MySQL queries provide the candidate lemmas for each WFR. Some of these queries run on the list of lemmas, while others on thelesarchive. In particular, most candidate lemmas of prefixal WFRs are found by running queries on the list of lemmas, as such rules tend to just add the characters of the prefix to the input lemma, like in the case ofaccuso→sub+accuso\(“to blame” → “to blame somewhat”\). Instead, suffixal WFRs are mostly assigned to their candidate input and output lemmas by running queries on thelesarchive, because suffixes attach tolesinstead of modifying full lemmas, like inamo→amabilis\(“to love” → “lovable”\) where suffix–bil–attaches tolesam\(plus the thematic vowel –a–, used for first conjugation verbs\) instead of full lemmaamo. Also, there are suffixal WFRs whose input is the basis of the irregular perfect participle of the input verb, like induco→ductilis\(“to lead” → “that may be led”\) where suffix–il–attaches to the basis of the irregular perfect participle of the verbduco\(duct\). Such irregular bases are recorded explicitly in the les archive with a specific codles.

**Making Choices**

Homography can occur at different levels when we search for pairings. Let us consider, for example, the application of prefixal rules to the list of lemmas. An SQL query searches through all verbs contained in the list of lemmas and is instructed to return them in two main groups: an input list of lemmas \(i\_lemma\) and an output list of lemmas \(o\_lemma\), where the output needs to look the same as the input with the addition of a string of characters, the prefix, preceding the same string of characters as it is displayed in the input. Such a query will return something like this:[\[1\]](applewebdata://D9774590-CC24-4FBC-B6A7-6E87FC15BDC1#_ftn1)



| **input\_lemma** | **output\_lemma** |
| :--- | :--- |
| sero 1 | subsero 1 |
| sero 1 | subsero 2 |
| sero 2 | subsero 1 |
| sero 2 | subsero 2 |
| sero 3 | subsero 1 |
| sero 3 | subsero 2 |



There are three lemmas _sero_ in Latin, with different inflections and meanings \(1 ‘to plant’, 2 ‘to entwine’ and 3 ‘to bolt’\), and two lemmas _subsero _\(1‘to plant as a replacement’ and 2 ‘to insert below’\). The simple pairing SQL query currently used does not discern which _subsero_ comes from which _sero_. However, there are two ways in which this can be solved. First, we can exclude the lemma _sero_ 3 as, unlike the other third conjugation lemmas, it is a first conjugation verb. The codles for both les _ser_ is v3r \(third conjugation regular verb\). All les related to a common lemma are grouped within the same clem \(“costellazione lemmatica”\) and referred to through the same identifier n\_id \(hereafter 1/2/3\). In the sero1 clem, for example, we have the forms _seu_ with codles v7s \(perfect\) and _sat_ with n6p1\(irregular perfect participles\); for _sero_ 2 we have _seru_ for v7r and _sert_ for n6p1; for _subsero_ 1 we have the forms _subseu_ with codles v7s and subsat n6p1; for _subsero_ 2 we have _subseru_ for v7r and _subsert_ for n6p1.

One can either write a query that looks deeper into the clem of a lemma to match lemmas on the basis of les for v7r and/or n6p1or make the distinction manually, in this case on the basis of inflection: sero\(1\), sevi, satus =&gt; subsero\(1\), subsevi, subsatus/ sero\(2\), serui, sertus =&gt; subsero \(2\), subserui, subsertus\).

At times, the disambiguation only works on meaning, as there might not be real formal differences between totally homographic lemmas \(i.e. lemmas that look completely identical even in their inflectional paradigms\). The only way to rectify these dubious pairs is to consult both the LEMLAT archive \(for additional morphological information\) and the two main reference dictionaries , _Oxford Latin Dictionary_ and _Georges and Georges_.

Homography happens even more frequently when the query performs searches directly onles, as in the case of suffixal rules. The probability that onelesmight look like another, even if the resulting lemma does not, becomes higher.

For example:



| **input\_lemma** |  | **output\_lemma** |  |
| :--- | :--- | :--- | :--- |
| 'uerna' | 1 | 'uernalis' | a |
| 'uerna' | 1 | 'uernalis' | b |
| 'uernum' | 2 | 'uernalis' | b |
| 'uernum' | 2 | 'uernalis' | a |



The pairings above are among the results of the query that pairs denominal adjectives of the second class with suffix -al with their supposed ‘root’ nouns \(N =&gt; A2\). As above, we have two identical entries for the adjective _uernalis_. This time the two adjectives are not distinguishable by any inflectional feature because their appearance is exactly the same as that in the LEMLAT database.

Only a dictionary consultation will reveal that the first entry for _uernalis_ \(a\) means ‘of spring’, while the second entry _uernalis_ \(b\) means ‘of or belonging to the house slave’. This allows one to establish that ‘a’ is connected to _uernum_ ‘spring’ \(2\) and ‘b’ is connected to _uerna_ ‘house slave’ \(1\).



Sometimes, deciding on which WFR to treat first can help reduce the manual work. Therefore, another factor that can affect the amount of manual checking is workflow efficiency.

Carefully choosing which WFRs to apply first is of crucial importance. As previously mentioned, there can be only one derivation relation for each output lemma. This allows one to exclude output lemmas that have already been paired, while building new relations. This can be very useful when working through the uncorrected outputs of very productive rules \(with thousands of parallels to check manually\), but it is only advantageous if the most productive rule has been treated first.

If all adjectives derived from the past participle of a verb through conversion, have conclusively been assigned, they will not turn up when processing another rule involving perhaps denominal adjectives that could be paired to other homographic roots.

Some morphotactically obscure word formation processes, such as certain kinds of compounding rules, are very difficult to formalise so that they can be found by automatic procedures. In these cases, derivations are added to the database almost entirely manually.

**Theoretical Issues**

As mentioned above, WFRs are conceived according to the Item-and-Arrangement model. This means that the construction of the lexicon favours a morpheme-based approach to morphology. Moreover, this model is put in practice through the use of input-output relationships between lexemes, in a morphotactic approach that has been prioritised over philological considerations. The use of directed edges in our derivational trees implies that one word formation process has happened before the other, and that one given lexeme specifically derives from another. The main issue in the development of a derivational morphology resource for a language like Latin - even though WFL focusses on Classical Latin - is that of the diachronic distribution of the lexicon, together with the fact that there are no native speakers to help with considerations about the transparency or opacity of a dubious derivation.

As a result, we are forced to commit to a clear-cut work policy in order to ensure consistency throughout the lexicon. Three major factors are considered when in doubt:

a\) theoretical statements \(Unitary Base Hypothesis,[\[1\]](#_ftn1) Item and Arrangement\), and previous research on word formation;

b\) dictionaries: Oxford Latin Dictionary and Georges and Georges, we consider whether or not they support our analysis;

c\) semantic motivations.

Overall coherence in the design of the lexicon is what generally drives decisions, but case-by-case analysis is necessary in order to establish a motivated derivation graph.

**4.1 Compounding or suffixation?**

In the treatment of certain lexemes, we had to take a decision on whether they are the result of compounding or whether certain second constituents need to be considered as suffixoids. For example, -fex -fico and -ficus \(from base form fec- from facio ‘to make’\) are considered suffixes denoting ‘making’ in Oxford Latin Dictionary. Yet, against Oxford Latin Dictionary’s position, we have chosen to consider -fex and -fico as second constituents for compounds, following authoritative bibliography on the subject such as Brucale 2012, Jenks 1911, Oniga 1988 and 1992.

For what -ficus \(again from facio, forming adjectives\) is concerned, on the other hand, we have taken a non-uniform approach: it was decided to treat certain lexemes ending in -ficus which had a corresponding verb ending in -fico, as V-to-N conversions instead. The reasoning behind this decision originated from the fact that, generally speaking, linguistic research on Latin word formation considers compounding - comparatively to what happens in other Indo-European languages - as a poorly productive phenomenon \(Fruyt 2002\). For this reason, one can imagine a different formation process to have happened instead. In the case of -fico and -ficus, for example, we hypothesise the verbal compound to have formed before the adjective, as the main meaning of such compounds is almost always the result of a performed action \(damnificus ‘that causes loss’ &lt;damnifico ‘to cause loss’ &lt;damnum + facio\). This also means that rather than having another separated compounding rule for damnificus, which would have connected the two lexemes at a higher, more distant, level \(from damnum and facio, but not with each other\), the two lexemes are directly connected in the same word formation family.

However, on 92 adjectives ending in -ficus contained in the lexical basis, 65 do not have a -fico verb counterpart to be connected to through conversion, hence they are considered the result of compounding. This has ultimately created an inconsistency in the resource, that needs to be either rectified or justified theoretically.

**4.2 Prefixation or conversion? Or ‘the chicken or the egg’ dilemma**

Occasionally, we had to decide whether to prioritise one derivation process over another. In certain cases, it is difficult to ascertain whether prefixation happened before or after conversion - or suffixation - diachronically. Let us consider, for example, the choice between the following two options: 1. sono ‘to make a noise’ &gt;resono ‘to produce a prolonged sound’ &gt;resonus ‘reverberating/echoing’, or 2. sono&gt;sonus ‘sounding’ &gt;resonus. In this case the derivation trail 1, is preferred over 2, as it absolves all three of the deciding factors outlined above. Generally speaking, conversion and prefixation could have happened at any point of the derivation process, but when trying to establish whether the prefixation process happened before or after certain conversive processes, we tend to give precedence to verbal prefixation, as it appears more productive \(Fruyt 2002, Oniga 1992\).[\[2\]](#_ftn2)Georges and Georges avails this hypothesis by stating that resonus comes from resono, and semantically the meaning of ‘reverberating/echoing’ seems to be more likely an inheritance from the verb meaning ‘to produce a prolonged sound’, rather than merely from the adjective ‘sounding’. In a similar way, supported by all or a combination of the three factors above, the derivation properus ‘quick’ &gt;propero ‘to hurry’&gt;depropero ‘to hasten’ &gt;deproperus ‘hastening’ is chosen over properus&gt;deproperus. On the other hand, notice how, for semantic reasons, sonus&gt;absonus ‘of unpleasant sound’ &gt;absono ‘to have an unpleasant sound’, as the semantic emphasis for the verbal counterpart here seems to derive from the adjective.

However, sometimes, when looking into prioritising prefixation over suffixation, relying on what the dictionary tells us makes things uneven within the same word formation family. See for example the case of horreo ‘to become stiff/ tremble \(with fear\) &gt;abhorreo ‘to recoil from’ &gt;abhorresco ‘to become disgusted’, but horreo&gt;horresco ‘to stand up stiffly / become agitated’ &gt;inhorresco ‘to become stiff \(with cold\)’ described in Budassi and Litta 2017.

**4.3 Fictional entries**

Another phenomenon that required an unconventional solution was the presence of parasynthetic phenomena formed through the simultaneous addition of a suffix and a prefix, or a prefix and conversion. See for example indolesco ‘to feel painful’, which, according to the Oxford Latin Dictionary, is formed by in + dolor + e-sco, or bicolor ‘of two colours’, an adjective resulting from prefixation of noun color ‘colour’ with prefix bi- indicating something ‘consisting of two things’. Bicolor has been inserted into WFL simply as the result of a prefixal WFR involving a change of PoS \(N-To-A\), while the first has created more complicated issues.

The project's morphotactic approach forced us to create a number of fictional lexemes in order to fill a gap in the derivation tree. These are not reconstructed lexemes, and are not expected to have ever existed, but only serve as a “mechanical” connection between two lexemes. For instance, \*indoleo only acts as a trait d'union between dolor ‘pain’, and indolesco ‘to feel painful’, hence creating two formative processes instead of one.

**4.4 Backformation**

Backformation is a derivation process that involves the removal of an affix \(or a supposed one\), so that a new word is created by analogy with similar looking existing ones, as for example galeo ‘to equip with a helmet’ &lt;galeatus \(adj.\) ‘wearing a helmet’, grego ‘to collect into a flock’ &lt;aggrego/congrego ‘to bring people together’, irascor ‘to be angry’ &lt;iratus ‘angy’, \(g\)nascor ‘to be born’ &lt;natus ‘born’.

At the time of writing, these and other backformation processes are not marked in WFL, but are portrayed as follows: galeo&lt;galea ‘helmet’, galeatus&lt;galea, aggrego/congrego&lt;grego, but iratus&gt;irascor \(A-To-V -sc-\), \(g\)nascor = root verb \(i.e. not derived\). In the future, we are planning on adding backformation as a de facto WFR, and mark backformations to the three graphs so that the edge can point in the opposite direction, or be recognisable by a different colour.

---

[\[1\]](#_ftnref1)According to the Unitary Base Hypothesis Word Formation Rules may only operate over a single type of syntactically or semantically defined base \(Scalise 1983, Aronoff 1976\).

[\[2\]](#_ftnref2)In WFL there are 4,281 prefixed V-To-V vs. 786 V-To-N and 101 V-To-A conversions.

 

