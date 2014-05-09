---
layout: default
title: STV
---

~~~
This model aims to model a single transferable voting system used for electronic 
P. Mukherjee and B.A. Wichmann. Formal Specification of the STV Algorithm. In
Paul Mukherjee,  Automatic translation of VDM-SL specifications into gofer, 
#******************************************************
~~~
###stv.vdmsl

{% raw %}
~~~
values
Number_of_vacancies = 5;
rand_choice = [<Bill>,<Adam>,<John>,<Frank>];
Votes = {
types
Candidate_names = <Adam>|<Bill>|<Charlie>|<Donald>|<Edward>|
Voting_paper = map Candidate_names to nat1
Parcel = map Voting_paper to nat1;
Score::name: Candidate_names
Stage = seq of Score
Value = real
Sub_parcel:: votes: Parcel
Candidate:: name: Candidate_names
Sub_parcel_bundle:: sub_parcels: map Candidate_names to Sub_parcel
Record_entry:: scores: set of Score
Result::scores: set of Score
Result_sheet:: results: seq of Result
Candset = set of Candidate;
state St of

functions
mult_p_sum:set of (nat * Parcel) -> nat
size: Parcel -> nat

disjoint: set of (set of Candidate) -> bool
vote_res: Parcel * set of Voting_paper -> Parcel
sort_papers: Parcel * set of Candidate_names -> Candset

two_decimal_places: real -> real
stage_bk: seq of Score -> Score
defer_transfer_of_surplus:real * Stage -> bool
sum: seq of real -> real
sole_leader: Stage * Candidate_names * set of Candidate_names -> bool
greatest_value_at_earliest_stage: Candidate_names * seq of Stage -> bool



surplus_from_original_votes: Candidate -> bool
construct_sub_parcels: Value * Parcel * Candidate * set of Candidate

non_transferable_papers: Parcel * Candidate_names * set of Candidate_names
next_preference: Candidate_names * Voting_paper * set of Candidate_names -> 
construct_bundle_for_transfer:real * Value * Parcel * Candidate * 

calc_transf_value: real * Value * Value * nat -> Value
calc_loss_of_value: real * Value * nat * Value -> real
calc_non_transf_value: real * Value -> Value
redistribute_parcels: Candset * Sub_parcel_bundle -> Candset
  score_sort: Stage -> Stage
  score_merge: Stage * Stage -> Stage
set_seq: set of Score -> Stage
build_first_stage: set of Candidate -> Stage

construct_new_stage:Stage * Candidate_names * Sub_parcel_bundle -> Stage
        union

exists_non_deferable_surplus: (seq of Stage) * real -> bool
trailing_candidate: Candidate_names * seq1 of Stage -> bool
sole_trailer: Stage * Candidate_names * set of Candidate_names -> bool


number_of_continuing_candidates: set of Candidate_names -> nat
number_of_remaining_vacancies: set of Candidate_names -> nat

total_value:Sub_parcel -> real
number_of_candidates_satisfying_quota: (set of Candidate) * (seq of Stage) * 
non_transferable_paper: Voting_paper * Candidate_names * 
min: set of real -> real
last_vacancy_fillable: (set of Candidate) * (seq of Stage) * real -> bool


make_result_sheet: seq of Stage * real * seq of Record_entry * 
sp_set_seq: set of Sub_parcel -> seq of Sub_parcel
  sub_parcels_sort: seq of Sub_parcel -> seq of Sub_parcel
  sub_parcels_merge: seq of Sub_parcel * seq of Sub_parcel -> seq of Sub_parcel

operations
CHOOSE_SURPLUS_TO_TRANSFER:() ==> Candidate_names
CHOOSE_CANDIDATE_TO_EXCLUDE:() ==> Candidate_names

RANDOM_ELEMENT:Candnset ==> Candidate_names
PREPARE_ELECTION: Parcel ==> ()

ELECT_ALL_REMAINING_CANDIDATES:() ==> ()

PROCESS_SUB_PARCELS:Candidate * seq of Sub_parcel ==> ()

ELECT_LAST_CANDIDATE:() ==> ()
TRANSFER_SURPLUS:() ==> ()

EXCLUDE_CANDIDATE:() ==> ()
ELECT_CANDIDATES:() ==> ()
CONDUCT_ELECTION: Parcel ==> Result_sheet
CHANGE_STATUS_OF_ELECTED_CANDIDATES:() ==> ()


~~~{% endraw %}
