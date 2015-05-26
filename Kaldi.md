
[TOC]

# 1. 

## 1. prepare data

```bash
# audio data preparation
# format the data as Kaldi data directories
for part in dev-clean test-clean train-clean-100; do
  # use underscore-separated names in data directories.
  local/data_prep.sh $data/LibriSpeech/$part data/$(echo $part | sed s/-/_/g) || exit 1
done
```

output:

```
utils/validate_data_dir.sh: Successfully validated data-directory data/dev_clean
local/data_prep.sh: successfully prepared data in data/dev_clean
utils/validate_data_dir.sh: Successfully validated data-directory data/test_clean
local/data_prep.sh: successfully prepared data in data/test_clean
utils/validate_data_dir.sh: Successfully validated data-directory data/train_clean_100
local/data_prep.sh: successfully prepared data in data/train_clean_100
```

## 2. prepare dict

```bash
# when "--stage 3" option is used below we skip the G2P steps, and use the
# lexicon we have already downloaded from openslr.org/11/
local/prepare_dict.sh --stage 3 --nj 30 --cmd "$train_cmd" \
   data/local/lm data/local/lm data/local/dict_nosp || exit 1
```

output:

```
Preparing phone lists and clustering questions
2 silence phones saved to: data/local/dict_nosp/silence_phones.txt
1 optional silence saved to: data/local/dict_nosp/optional_silence.txt
39 non-silence phones saved to: data/local/dict_nosp/nonsilence_phones.txt
5 extra triphone clustering-related questions saved to: data/local/dict_nosp/extra_questions.txt
Lexicon text file saved as: data/local/dict_nosp/lexicon.txt
```

## 3. prepare language

```bash
utils/prepare_lang.sh data/local/dict_nosp \
  "<SPOKEN_NOISE>" data/local/lang_tmp_nosp data/lang_nosp || exit 1;
```

output:

```
Checking data/local/dict_nosp/silence_phones.txt ...
--> reading data/local/dict_nosp/silence_phones.txt
--> data/local/dict_nosp/silence_phones.txt is OK

Checking data/local/dict_nosp/optional_silence.txt ...
--> reading data/local/dict_nosp/optional_silence.txt
--> data/local/dict_nosp/optional_silence.txt is OK

Checking data/local/dict_nosp/nonsilence_phones.txt ...
--> reading data/local/dict_nosp/nonsilence_phones.txt
--> data/local/dict_nosp/nonsilence_phones.txt is OK

Checking disjoint: silence_phones.txt, nonsilence_phones.txt
--> disjoint property is OK.

Checking data/local/dict_nosp/lexicon.txt
--> reading data/local/dict_nosp/lexicon.txt
--> data/local/dict_nosp/lexicon.txt is OK

Checking data/local/dict_nosp/extra_questions.txt ...
--> reading data/local/dict_nosp/extra_questions.txt
--> data/local/dict_nosp/extra_questions.txt is OK
--> SUCCESS [validating dictionary directory data/local/dict_nosp]

**Creating data/local/dict_nosp/lexiconp.txt from data/local/dict_nosp/lexicon.txt

fstaddselfloops 'echo 347 |' 'echo 200004 |'
prepare_lang.sh: validating output directory
Checking data/lang_nosp/phones.txt ...
--> data/lang_nosp/phones.txt is OK

Checking words.txt: #0 ...
--> data/lang_nosp/words.txt has "#0"
--> data/lang_nosp/words.txt is OK

Checking disjoint: silence.txt, nonsilence.txt, disambig.txt ...
--> silence.txt and nonsilence.txt are disjoint
--> silence.txt and disambig.txt are disjoint
--> disambig.txt and nonsilence.txt are disjoint
--> disjoint property is OK

Checking sumation: silence.txt, nonsilence.txt, disambig.txt ...
--> summation property is OK

Checking data/lang_nosp/phones/context_indep.{txt, int, csl} ...
--> 10 entry/entries in data/lang_nosp/phones/context_indep.txt
--> data/lang_nosp/phones/context_indep.int corresponds to data/lang_nosp/phones/context_indep.txt
--> data/lang_nosp/phones/context_indep.csl corresponds to data/lang_nosp/phones/context_indep.txt
--> data/lang_nosp/phones/context_indep.{txt, int, csl} are OK

Checking data/lang_nosp/phones/disambig.{txt, int, csl} ...
--> 17 entry/entries in data/lang_nosp/phones/disambig.txt
--> data/lang_nosp/phones/disambig.int corresponds to data/lang_nosp/phones/disambig.txt
--> data/lang_nosp/phones/disambig.csl corresponds to data/lang_nosp/phones/disambig.txt
--> data/lang_nosp/phones/disambig.{txt, int, csl} are OK

Checking data/lang_nosp/phones/nonsilence.{txt, int, csl} ...
--> 336 entry/entries in data/lang_nosp/phones/nonsilence.txt
--> data/lang_nosp/phones/nonsilence.int corresponds to data/lang_nosp/phones/nonsilence.txt
--> data/lang_nosp/phones/nonsilence.csl corresponds to data/lang_nosp/phones/nonsilence.txt
--> data/lang_nosp/phones/nonsilence.{txt, int, csl} are OK

Checking data/lang_nosp/phones/silence.{txt, int, csl} ...
--> 10 entry/entries in data/lang_nosp/phones/silence.txt
--> data/lang_nosp/phones/silence.int corresponds to data/lang_nosp/phones/silence.txt
--> data/lang_nosp/phones/silence.csl corresponds to data/lang_nosp/phones/silence.txt
--> data/lang_nosp/phones/silence.{txt, int, csl} are OK

Checking data/lang_nosp/phones/optional_silence.{txt, int, csl} ...
--> 1 entry/entries in data/lang_nosp/phones/optional_silence.txt
--> data/lang_nosp/phones/optional_silence.int corresponds to data/lang_nosp/phones/optional_silence.txt
--> data/lang_nosp/phones/optional_silence.csl corresponds to data/lang_nosp/phones/optional_silence.txt
--> data/lang_nosp/phones/optional_silence.{txt, int, csl} are OK

Checking data/lang_nosp/phones/roots.{txt, int} ...
--> 41 entry/entries in data/lang_nosp/phones/roots.txt
--> data/lang_nosp/phones/roots.int corresponds to data/lang_nosp/phones/roots.txt
--> data/lang_nosp/phones/roots.{txt, int} are OK

Checking data/lang_nosp/phones/sets.{txt, int} ...
--> 41 entry/entries in data/lang_nosp/phones/sets.txt
--> data/lang_nosp/phones/sets.int corresponds to data/lang_nosp/phones/sets.txt
--> data/lang_nosp/phones/sets.{txt, int} are OK

Checking data/lang_nosp/phones/extra_questions.{txt, int} ...
--> 14 entry/entries in data/lang_nosp/phones/extra_questions.txt
--> data/lang_nosp/phones/extra_questions.int corresponds to data/lang_nosp/phones/extra_questions.txt
--> data/lang_nosp/phones/extra_questions.{txt, int} are OK

Checking data/lang_nosp/phones/word_boundary.{txt, int} ...
--> 346 entry/entries in data/lang_nosp/phones/word_boundary.txt
--> data/lang_nosp/phones/word_boundary.int corresponds to data/lang_nosp/phones/word_boundary.txt
--> data/lang_nosp/phones/word_boundary.{txt, int} are OK

Checking optional_silence.txt ...
--> reading data/lang_nosp/phones/optional_silence.txt
--> data/lang_nosp/phones/optional_silence.txt is OK

Checking disambiguation symbols: #0 and #1
--> data/lang_nosp/phones/disambig.txt has "#0" and "#1"
--> data/lang_nosp/phones/disambig.txt is OK

Checking topo ...
--> data/lang_nosp/topo's nonsilence section is OK
--> data/lang_nosp/topo's silence section is OK
--> data/lang_nosp/topo is OK

Checking word_boundary.txt: silence.txt, nonsilence.txt, disambig.txt ...
--> data/lang_nosp/phones/word_boundary.txt doesn't include disambiguation symbols
--> data/lang_nosp/phones/word_boundary.txt is the union of nonsilence.txt and silence.txt
--> data/lang_nosp/phones/word_boundary.txt is OK

Checking word_boundary.int and disambig.int
--> generating a 12 word sequence
--> resulting phone sequence from L.fst corresponds to the word sequence
--> L.fst is OK
--> generating a 81 word sequence
--> resulting phone sequence from L_disambig.fst corresponds to the word sequence
--> L_disambig.fst is OK

Checking data/lang_nosp/oov.{txt, int} ...
--> 1 entry/entries in data/lang_nosp/oov.txt
--> data/lang_nosp/oov.int corresponds to data/lang_nosp/oov.txt
--> data/lang_nosp/oov.{txt, int} are OK

--> data/lang_nosp/L.fst is olabel sorted
--> data/lang_nosp/L_disambig.fst is olabel sorted
--> SUCCESS [validating lang directory data/lang_nosp]
```

## 4. format lms

```bash
local/format_lms.sh --src-dir data/lang_nosp data/local/lm || exit 1
```

output: missing

## 5. Create ConstArpaLm format language model for full 3-gram and 4-gram LMs

```bash
# Create ConstArpaLm format language model for full 3-gram and 4-gram LMs
utils/build_const_arpa_lm.sh data/local/lm/lm_tglarge.arpa.gz \
  data/lang_nosp data/lang_nosp_test_tglarge || exit 1;
utils/build_const_arpa_lm.sh data/local/lm/lm_fglarge.arpa.gz \
  data/lang_nosp data/lang_nosp_test_fglarge || exit 1;
```

output:

```
arpa-to-const-arpa --bos-symbol=200005 --eos-symbol=200006 --unk-symbol=2 'gunzip -c data/local/lm/lm_tglarge.arpa.gz | utils/map_arpa_lm.pl data/lang_nosp_test_tglarge/words.txt|' data/lang_nosp_test_tglarge/G.carpa
utils/map_arpa_lm.pl: Processing "\data\"
utils/map_arpa_lm.pl: Processing "\1-grams:\"
LOG (arpa-to-const-arpa:Read():const-arpa-lm.cc:310) Reading "\data\" section.
LOG (arpa-to-const-arpa:Read():const-arpa-lm.cc:363) Reading "\1-grams:" section.
utils/map_arpa_lm.pl: Processing "\2-grams:\"
LOG (arpa-to-const-arpa:Read():const-arpa-lm.cc:363) Reading "\2-grams:" section.
utils/map_arpa_lm.pl: Processing "\3-grams:\"
LOG (arpa-to-const-arpa:Read():const-arpa-lm.cc:363) Reading "\3-grams:" section.
arpa-to-const-arpa --bos-symbol=200005 --eos-symbol=200006 --unk-symbol=2 'gunzip -c data/local/lm/lm_fglarge.arpa.gz | utils/map_arpa_lm.pl data/lang_nosp_test_fglarge/words.txt|' data/lang_nosp_test_fglarge/G.carpa
utils/map_arpa_lm.pl: Processing "\data\"
utils/map_arpa_lm.pl: Processing "\1-grams:\"
LOG (arpa-to-const-arpa:Read():const-arpa-lm.cc:310) Reading "\data\" section.
LOG (arpa-to-const-arpa:Read():const-arpa-lm.cc:363) Reading "\1-grams:" section.
utils/map_arpa_lm.pl: Processing "\2-grams:\"
LOG (arpa-to-const-arpa:Read():const-arpa-lm.cc:363) Reading "\2-grams:" section.
utils/map_arpa_lm.pl: Processing "\3-grams:\"
LOG (arpa-to-const-arpa:Read():const-arpa-lm.cc:363) Reading "\3-grams:" section.
utils/map_arpa_lm.pl: Processing "\4-grams:\"
LOG (arpa-to-const-arpa:Read():const-arpa-lm.cc:363) Reading "\4-grams:" section.
```

## 6. generate mfcc features

```bash
mfccdir=mfcc

for part in dev_clean test_clean dev_other test_other train_clean_100; do
  steps/make_mfcc.sh --cmd "$train_cmd" --nj 40 data/$part exp/make_mfcc/$part $mfccdir
  steps/compute_cmvn_stats.sh data/$part exp/make_mfcc/$part $mfccdir
done
```

output:

```
steps/make_mfcc.sh --cmd run.pl --nj 40 data/dev_clean exp/make_mfcc/dev_clean mfcc
utils/validate_data_dir.sh: Successfully validated data-directory data/dev_clean
steps/make_mfcc.sh: [info]: no segments file exists: assuming wav.scp indexed by utterance.
Succeeded creating MFCC features for dev_clean
steps/compute_cmvn_stats.sh data/dev_clean exp/make_mfcc/dev_clean mfcc
Succeeded creating CMVN stats for dev_clean
steps/make_mfcc.sh --cmd run.pl --nj 40 data/test_clean exp/make_mfcc/test_clean mfcc
utils/validate_data_dir.sh: Successfully validated data-directory data/test_clean
steps/make_mfcc.sh: [info]: no segments file exists: assuming wav.scp indexed by utterance.
Succeeded creating MFCC features for test_clean
steps/compute_cmvn_stats.sh data/test_clean exp/make_mfcc/test_clean mfcc
Succeeded creating CMVN stats for test_clean
steps/make_mfcc.sh --cmd run.pl --nj 40 data/train_clean_100 exp/make_mfcc/train_clean_100 mfcc
utils/validate_data_dir.sh: Successfully validated data-directory data/train_clean_100
steps/make_mfcc.sh: [info]: no segments file exists: assuming wav.scp indexed by utterance.
Succeeded creating MFCC features for train_clean_100
steps/compute_cmvn_stats.sh data/train_clean_100 exp/make_mfcc/train_clean_100 mfcc
Succeeded creating CMVN stats for train_clean_100
```

## 7. select subset of data for early stages system builds

```bash
# Make some small data subsets for early system-build stages.  Note, there are 29k
# utterances in the train_clean_100 directory which has 100 hours of data.
# For the monophone stages we select the shortest utterances, which should make it
# easier to align the data from a flat start.

utils/subset_data_dir.sh --shortest data/train_clean_100 2000 data/train_2kshort
utils/subset_data_dir.sh data/train_clean_100 5000 data/train_5k
utils/subset_data_dir.sh data/train_clean_100 10000 data/train_10k
```

output:

```
utils/subset_data_dir.sh: reducing #utt from 28539 to 2000
utils/subset_data_dir.sh: reducing #utt from 28539 to 5000
utils/subset_data_dir.sh: reducing #utt from 28539 to 10000
```

## 8. train monophone system

```bash
# train a monophone system
steps/train_mono.sh --boost-silence 1.25 --nj 20 --cmd "$train_cmd" \
  data/train_2kshort data/lang_nosp exp/mono || exit 1;
```

output:

```
steps/train_mono.sh --boost-silence 1.25 --nj 20 --cmd run.pl data/train_2kshort data/lang_nosp exp/mono
steps/train_mono.sh: Initializing monophone system.
steps/train_mono.sh: Compiling training graphs
steps/train_mono.sh: Aligning data equally (pass 0)
steps/train_mono.sh: Pass 1
steps/train_mono.sh: Aligning data
...
steps/train_mono.sh: Pass 39
626 warnings in exp/mono/log/align.*.*.log
Done
```

## 9. decode using the monophone model

```bash
# decode using the monophone model
(
  utils/mkgraph.sh --mono data/lang_nosp_test_tgsmall \
    exp/mono exp/mono/graph_nosp_tgsmall || exit 1
  for test in test_clean test_other dev_clean dev_other; do
    steps/decode.sh --nj 20 --cmd "$decode_cmd" exp/mono/graph_nosp_tgsmall \
      data/$test exp/mono/decode_nosp_tgsmall_$test || exit 1
  done
)&

steps/align_si.sh --boost-silence 1.25 --nj 10 --cmd "$train_cmd" \
  data/train_5k data/lang_nosp exp/mono exp/mono_ali_5k
```

output:

```
steps/align_si.sh --bofsttablecompose data/lang_nosp_test_tgsmall/L_disambig.fst data/lang_nosp_test_tgsmall/G.fststeps/align_si.sh: feature type is delta
steps/align_si.sfstisstochastic data/lang_nosp_test_tgsmall/tmp/LG.fst
fstcomposecontext --context-size=1 --central-position=0 --read-disambig-syms=data/lang_nosp_test_tgsmall/phones/disambig.int --wri0.000487456 -1.38295
[info]: CLG not stochastic.
0.000845104 -1.38281
HCLGa is not stochastic
steps/decode.sh --nj 20 --cmd run.pl exp/mono/graph_nosp_tgsmall data/test_clean exp/mono/decode_nosp_tgsmall_test_clean
decode.sh: feature type is delta
steps/decode.sh --nj 20 --cmd run.pl exp/mono/graph_nosp_tgsmall data/dev_clean exp/mono/decode_nosp_tgsmall_dev_clean
decode.sh: feature type is delta
tgsmall/Ha.fst data/lang_nosp_test_tgsmall/tmp/CLG_1_0.fst
fstdeterminizestar --use-log=true
fstminimizeencoded
fstrmsymbols exp/mono/graph_nosp_tgsmall/disambig_tid.int
fstrmepslocal
fstisstochastic exp/mono/graph_nosp_tgsmall/HCLGa.fst
add-self-loops --self-loop-scale=0.1 --reorder=true exp/mono/final.mdl
```

## 10. training and decoding using tri-phone model

```bash
# train a first delta + delta-delta triphone system on a subset of 5000 utterances
steps/train_deltas.sh --boost-silence 1.25 --cmd "$train_cmd" \
    2000 10000 data/train_5k data/lang_nosp exp/mono_ali_5k exp/tri1 || exit 1;

# decode using the tri1 model
(
  utils/mkgraph.sh data/lang_nosp_test_tgsmall \
    exp/tri1 exp/tri1/graph_nosp_tgsmall || exit 1;
  for test in test_clean test_other dev_clean dev_other; do
    steps/decode.sh --nj 20 --cmd "$decode_cmd" exp/tri1/graph_nosp_tgsmall \
      data/$test exp/tri1/decode_nosp_tgsmall_$test || exit 1;
    steps/lmrescore.sh --cmd "$decode_cmd" data/lang_nosp_test_{tgsmall,tgmed} \
      data/$test exp/tri1/decode_nosp_{tgsmall,tgmed}_$test  || exit 1;
    steps/lmrescore_const_arpa.sh \
      --cmd "$decode_cmd" data/lang_nosp_test_{tgsmall,tglarge} \
      data/$test exp/tri1/decode_nosp_{tgsmall,tglarge}_$test || exit 1;
  done
)&
```

output:

```
```

## 11. training and decoding using LDA+MLLT

```bash
steps/align_si.sh --nj 10 --cmd "$train_cmd" \
  data/train_10k data/lang_nosp exp/tri1 exp/tri1_ali_10k || exit 1;


# train an LDA+MLLT system.
steps/train_lda_mllt.sh --cmd "$train_cmd" \
   --splice-opts "--left-context=3 --right-context=3" 2500 15000 \
   data/train_10k data/lang_nosp exp/tri1_ali_10k exp/tri2b || exit 1;

# decode using the LDA+MLLT model
(
  utils/mkgraph.sh data/lang_nosp_test_tgsmall \
    exp/tri2b exp/tri2b/graph_nosp_tgsmall || exit 1;
  for test in test_clean test_other dev_clean dev_other; do
    steps/decode.sh --nj 20 --cmd "$decode_cmd" exp/tri2b/graph_nosp_tgsmall \
      data/$test exp/tri2b/decode_nosp_tgsmall_$test || exit 1;
    steps/lmrescore.sh --cmd "$decode_cmd" data/lang_nosp_test_{tgsmall,tgmed} \
      data/$test exp/tri2b/decode_nosp_{tgsmall,tgmed}_$test  || exit 1;
    steps/lmrescore_const_arpa.sh \
      --cmd "$decode_cmd" data/lang_nosp_test_{tgsmall,tglarge} \
      data/$test exp/tri2b/decode_nosp_{tgsmall,tglarge}_$test || exit 1;
  done
)&
```

output:

```
steps/align_si.sh --njrm: cannot remove `exp/tri2b/lda.*.acc': No such file or directory
make-h-transducer --disambig-syms-out=exp/tri2b/graph_nosp_tgsmall/disambig_tid.int --transition-scale=1.0 data/lang_nosp_test_tgsmall/tmp/ilabels_3_1 exp/tri2b/tree exp/tri2b/final.mdl
fstminimizeencoded
fstdeterminizestar --use-log=true
fstisstochastic exp/tri2b/graph_nosp_tgsmall/HCLGa.fst
add-self-loops --self-loop-scale=0.1 --reorder=true exp/tri2b/final.mdl
DA statistics.
Accumulating tree stats
Getting questions for tree clustering.
Building the tree
steps/train_lda_mllt.sh: Initializing the model
Converting alignments from exp/tri1_ali_10k to use current tree
Compiling graphs of transcripts
Training pass 1
Training pass 2
Estimating MLLT
...
Training pass 34
1 warnings in exp/tri2b/log/build_tree.log
138 warnings in exp/tri2b/log/align.*.*.log
1 warnings in exp/tri2b/log/compile_questions.log
Done training system with LDA+MLLT features in exp/tri2b
0.00094317 -1.38286
HCLGa is not stochastic
steps/decode.sh --nj 20 --cmd run.pl exp/tri2b/graph_nosp_tgsmall data/test_clean exp/tri2b/decode_nosp_tgsmall_test_clean
decode.sh: feature type is lda
```

## 12. training and decoding using LDA+MLLT+SAT

```bash
# Align a 10k utts subset using the tri2b model
steps/align_si.sh  --nj 10 --cmd "$train_cmd" --use-graphs true \
  data/train_10k data/lang_nosp exp/tri2b exp/tri2b_ali_10k || exit 1;

# Train tri3b, which is LDA+MLLT+SAT on 10k utts
steps/train_sat.sh --cmd "$train_cmd" 2500 15000 \
  data/train_10k data/lang_nosp exp/tri2b_ali_10k exp/tri3b || exit 1;

# decode using the tri3b model
(
  utils/mkgraph.sh data/lang_nosp_test_tgsmall \
    exp/tri3b exp/tri3b/graph_nosp_tgsmall || exit 1;
  for test in test_clean test_other dev_clean dev_other; do
    steps/decode_fmllr.sh --nj 20 --cmd "$decode_cmd" \
      exp/tri3b/graph_nosp_tgsmall data/$test \
      exp/tri3b/decode_nosp_tgsmall_$test || exit 1;
    steps/lmrescore.sh --cmd "$decode_cmd" data/lang_nosp_test_{tgsmall,tgmed} \
      data/$test exp/tri3b/decode_nosp_{tgsmall,tgmed}_$test  || exit 1;
    steps/lmrescore_const_arpa.sh \
      --cmd "$decode_cmd" data/lang_nosp_test_{tgsmall,tglarge} \
      data/$test exp/tri3b/decode_nosp_{tgsmall,tglarge}_$test || exit 1;
  done
)&
```

output:

```
steps/align_si.sh --njmake-h-transducer --disambig-syms-out=exp/tri3b/graph_nosp_tgsmall/disambig_tid.int --transition-scale=1.0 data/lang_nosp_test_tgsmall/tmp/ilabels_3_1 exp/tri3b/tree exp/tri3b/final.mdl
fstrmsymbols exp/tri3b/graph_nosp_tgsmall/disambig_tid.int
fstdeterminizestar --use-log=true
fstminimizeencoded
fsttablecompose exp/tri3b/graph_nosp_tgsmall/Ha.fst data/lang_nosp_test_tgsmall/tmp/CLG_3_1.fst
fstisstochastic exp/tri3b/graph_nosp_tgsmall/HCLGa.fst
add-self-loops --self-loop-scale=0.1 --reorder=true exp/tri3b/final.mdl
fstdeterminizestar
fstrmsymbols data/lang_nosp_test_tgmed/phones/disambig.int
cat: data/test_other/utt2spk: No such file or directory
e
steps/train_sat.sh: Initializing the model
steps/train_sat.sh: Converting alignments from exp/tri2b_ali_10k to use current tree
steps/train_sat.sh: Compiling graphs of transcripts
Pass 1
Pass 2
Estimating fMLLR transforms
Pass 3
Pass 4
Estimating fMLLR transforms
...
Pass 10
Aligning data
...
Pass 34
153 warnings in exp/tri3b/log/align.*.*.log
1 warnings in exp/tri3b/log/build_tree.log
1 warnings in exp/tri3b/log/compile_questions.log
steps/train_sat.sh: Likelihood evolution:
-49.6795 -49.3683 -49.2625 -49.1656 -48.6369 -48.1089 -47.7731 -47.5591 -47.4127 -46.919 -46.7486 -46.567 -46.4613 -46.3732 -46.2913 -46.22 -46.1564 -46.0956 -46.037 -45.9017 -45.8251 -45.7791 -45.7365 -45.6952 -45.6563 -45.6193 -45.5844 -45.5507 -45.5171 -45.4388 -45.3916 -45.3694 -45.3547 -45.3426
Done
0.00092395 -1.38304
HCLGa is not stochastic
steps/decode_fmllr.sh --nj 20 --cmd run.pl exp/tri3b/graph_nosp_tgsmall data/test_clean exp/tri3b/decode_nosp_tgsmall_test_clean
steps/decode.sh --parallel-opts  --scoring-opts  --num-threads 1 --skip-scoring false --acwt 0.083333 --nj 20 --cmd run.pl --beam 10.0 --model exp/tri3b/final.alimdl --max-active 2000 exp/tri3b/graph_nosp_tgsmall data/test_clean exp/tri3b/decode_nosp_tgsmall_test_clean.si
decode.sh: feature type is lda
steps/decode_fmllr.sh: feature type is lda
steps/decode_fmllr.sh: getting first-pass fMLLR transforms.
steps/decode_fmllr.sh: doing main lattice generation phase
steps/decode_fmllr.sh: estimating fMLLR transforms a second time.
steps/decode_fmllr.sh: doing a final pass of acoustic rescoring.
steps/lmrescore.sh --cmd run.pl data/lang_nosp_test_tgsmall data/lang_nosp_test_tgmed data/test_clean exp/tri3b/decode_nosp_tgsmall_test_clean exp/tri3b/decode_nosp_tgmed_test_clean
steps/lmrescore_const_arpa.sh --cmd run.pl data/lang_nosp_test_tgsmall data/lang_nosp_test_tglarge data/test_clean exp/tri3b/decode_nosp_tgsmall_test_clean exp/tri3b/decode_nosp_tglarge_test_clean
steps/decode_fmllr.sh --nj 20 --cmd run.pl exp/tri3b/graph_nosp_tgsmall data/test_other exp/tri3b/decode_nosp_tgsmall_test_other
steps/decode_fmllr.sh: no such file data/test_other/feats.scp
```

# 2. Kaldi IO mechanisms

## Basics

standard interface:

```cpp
class SomeKaldiClass {
 public:
   void Read(std::istream &is, bool binary);
   void Write(std::ostream &os, bool binary) const;
};

class SomeKaldiClass {
 public:
  void Read(std::istream &is, bool binary, bool add = false);
};

// we suppose that class_member_ is of type int32.
void SomeKaldiClass::Read(std::istream &is, bool binary) {
  ReadBasicType(binary, &class_member_);
}
void SomeKaldiClass::Write(std::ostream &is, bool binary) const {
  WriteBasicType(binary, class_member_); 
}
```

If add==true, the Read function would add whatever is on disk (e.g. statistics) to the current class's contents, if the class is not currently empty.

- Size of basic type should be known in complie time, but floating types can be automatically converted. In **binary** format file, a character is used to represent sign and size of the basic type value. 
- **No byte swapping**

```cpp
void ReadToken(std::istream &is, bool binary, std::string *token);
void WriteToken(std::ostream &os, bool binary, const std::string & token);
```

- a token is an non-empty string with no spaces, eg. "<phone>"
- ExpectToken is similar to ReadToken, except you give it the string you expect (and it will throw an exception if it doesn't get it).

## How kaldi objects are stored in files

