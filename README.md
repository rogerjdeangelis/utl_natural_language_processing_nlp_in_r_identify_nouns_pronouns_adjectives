# utl_natural_language_processing_nlp_in_r_identify_nouns_pronouns_adjectives
Natural Language Processing NLP in R Identify Nouns Pronouns Adjectives.  Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.
    Natural Language Processing NLP in R Identify Nouns Pronouns Adjectives

    https://tinyurl.com/y9g2zxuw
    https://github.com/rogerjdeangelis/utl_natural_language_processing_nlp_in_r_identify_nouns_pronouns_adjectives

    stackoverflow
    https://stackoverflow.com/questions/51861346/r-pos-tagging-and-tokenizing-in-one-go

    INPUT
    =====

     SD1.HAVE total obs=1
                                    TXT

     Pierre Vinken, 61 years old, will join the board as a nonexecutive director
     Nov. 29.\n Mr. Vinken is chairman of Elsevier N.V., the Dutch publishing group.


     proc format;
      value $tok2grammer
         "CC  "="Coordinating conjunction                "
         "CD  "="Cardinal number                         "
         "DT  "="Determiner                              "
         "EX  "="Existential there                       "
         "FW  "="Foreign word                            "
         "IN  "="Preposition or subordinating conjunction"
         "JJ  "="Adjective                               "
         "JJR "="Adjective, comparative                  "
         "JJS "="Adjective, superlative                  "
         "LS  "="List item marker                        "
         "MD  "="Modal                                   "
         "NN  "="Noun, singular or mass                  "
         "NNS "="Noun, plural                            "
         "NNP "="Proper noun, singular                   "
         "NNPS"="Proper noun, plural                     "
         "PDT "="Predeterminer                           "
         "POS "="Possessive ending                       "
         "PRP "="Personal pronoun                        "
         "PRP$"="Possessive pronoun                      "
         "RB  "="Adverb                                  "
         "RBR "="Adverb, comparative                     "
         "RBS "="Adverb, superlative                     "
         "RP  "="Particle                                "
         "SYM "="Symbol                                  "
         "UH  "="Interjection                            "
         "VB  "="Verb, base form                         "
         "VBD "="Verb, past tense                        "
         "VBG "="Verb, gerund or present participle      "
         "VBN "="Verb, past participle                   "
         "VBP "="Verb, non­3rd person singular present   "
         "VBZ "="Verb, 3rd person singular present       "
         "WDT "="Wh­determiner                           "
         "WP  "="Wh­pronoun                              "
         "WP$ "="Possessive wh­pronoun                   "
         "WRB "="Wh­adverb                               "
         other="Punctuation"
     ;run;quit;


     EXAMPLE OUTPUT
     --------------

     WORK.WANT total obs=31

          WORD           GRAMMER

         Pierre          Proper noun, singular
         Vinken          Proper noun, singular
         ,               Punctuation
         61              Cardinal number
         years           Noun, plural
         old             Adjective
         ,               Punctuation
         will            Modal
         join            Verb, base form
         the             Determiner
         board           Noun, singular or mass
         as              Preposition or subordinating conjunction
         a               Determiner
         nonexecutive    Adjective
         director        Noun, singular or mass
         Nov.            Proper noun, singular
         29              Cardinal number
         .               Punctuation
         Mr.             Proper noun, singular
         Vinken          Proper noun, singular
         is              Verb, 3rd person singular present
         chairman        Noun, singular or mass
         of              Preposition or subordinating conjunction
         Elsevier        Proper noun, singular
         N.V.            Proper noun, singular
         ,               Punctuation
         the             Determiner
         Dutch           Adjective
         publishing      Noun, singular or mass
         group           Noun, singular or mass
         .               Punctuation

    PROCESS
    =======

    %utl_submit_r64('
    library(stringr);
    library(NLP);
    library(openNLP);
    library(haven);
    txt<-read_sas("d:/sd1/txt.sas7bdat");
    txt;
    s <- as.String(txt$TXT);
    sent_token_annotator <- Maxent_Sent_Token_Annotator();
    word_token_annotator <- Maxent_Word_Token_Annotator();
    a2 <- annotate(s, list(sent_token_annotator, word_token_annotator));
    pos_tag_annotator <- Maxent_POS_Tag_Annotator();
    pos_tag_annotator;
    a3 <- annotate(s, pos_tag_annotator, a2);
    a3;
    head(annotate(s, Maxent_POS_Tag_Annotator(probs = TRUE), a2));
    a3w <- subset(a3, type == "word");
    tags <- sapply(a3w$features, `[[`, "POS");
    tags;
    table(tags);
    want<-as.data.frame(sprintf("%s/%s", s[a3w], tags));
    colnames(want)<-"wrdGrm";
    want;
    save.image("d:/rda/work.Rdata");
    ');

    * create sas dataset from R dataframe;
    proc iml;
      submit / R;
      load("d:/rda/work.Rdata");
      want;
      endsubmit;
      run importdatasetfromr("work.want","want");
    run;quit;

    data WANT;
      set want;
      word=scan(wrdGrm,1,'/');
      grm=scan(wrdGrm,2,'/');
      grammer=put(strip(grm),$tok2grammer.);
      keep word grammer;
    run;quit;


    OUTPUT
    ======

     WORK.WANT total obs=31

          WORD           GRAMMER

         Pierre          Proper noun, singular
         Vinken          Proper noun, singular
         ,               Punctuation
         61              Cardinal number
         years           Noun, plural
         old             Adjective
         ,               Punctuation
         will            Modal
         join            Verb, base form
         the             Determiner
         board           Noun, singular or mass
         as              Preposition or subordinating conjunction
         a               Determiner
         nonexecutive    Adjective
         director        Noun, singular or mass
         Nov.            Proper noun, singular
         29              Cardinal number
         .               Punctuation
         Mr.             Proper noun, singular
         Vinken          Proper noun, singular
         is              Verb, 3rd person singular present
         chairman        Noun, singular or mass
         of              Preposition or subordinating conjunction
         Elsevier        Proper noun, singular
         N.V.            Proper noun, singular
         ,               Punctuation
         the             Determiner
         Dutch           Adjective
         publishing      Noun, singular or mass
         group           Noun, singular or mass
         .               Punctuation


