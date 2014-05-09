---
layout: default
title: loose
---

~~~
This VDM model is made by Peter Gorm Larsen as an exploration of how
Peter Gorm Larsen, Evaluation of Underdetermined Explicit Expressions,
#******************************************************
~~~
###as.vdmsl

{% raw %}
~~~



-----------------------------------------------------------------------
types

-----------------------------------------------------------------------
Definitions :: valuem : seq of ValueDef


-----------------------------------------------------------------------
ValueDef :: pat : Pattern                                     
-----------------------------------------------------------------------
ExplFnDef :: nm      : Name
-----------------------------------------------------------------------
Expr = LetExpr | LetBeSTExpr| IfExpr | CasesExpr |

BracketedExpr :: expr : Expr;
LetExpr :: lhs   : Pattern
LetBeSTExpr :: lhs : Bind                                     
IfExpr :: test   : Expr                                          
CasesExpr :: sel    : Expr
CaseAltn :: match : Pattern
UnaryExpr  :: opr : UnaryOp
UnaryOp = <NUMMINUS>;
BinaryExpr :: left  : Expr
BinaryOp = <EQ> | <NUMPLUS> | <NUMMINUS> | <NUMMULT> | <SETMINUS> ;
SetEnumerationExpr :: els : seq of Expr;
ApplyExpr :: fct : Name
Name :: ids : seq of Id;
Id = seq of char;
-----------------------------------------------------------------------
Pattern = PatternName | MatchVal | SetPattern;
PatternName :: nm : [(Name * Position)];
MatchVal :: val : Expr;
SetPattern = SetEnumPattern | SetUnionPattern;
SetEnumPattern :: Elems : seq of Pattern;
SetUnionPattern :: lp : Pattern
Position = nat * nat;
Bind = SetBind;
SetBind :: pat : Pattern
-----------------------------------------------------------------------
Literal = BoolLit | NumLit;
BoolLit:: val : bool;
NumLit :: val : int
values
 pat : Pattern = mk_PatternName(mk_(mk_Name(["x"]),mk_(1,1)));
 sexpr : Expr = mk_SetEnumerationExpr([mk_NumLit(1),mk_NumLit(2)]);
 expr2 : Expr = mk_BinaryExpr(expr, <NUMPLUS>, expr);


~~~{% endraw %}

###auxil.vdmsl

{% raw %}
~~~


operations
SeqOfSetOf2SetOfSeqOf : seq of set of (VAL | BlkEnv) ==> 
functions
  Consistent: LVAL * Model -> LVAL
  SetToSeq: set of VAL +> seq of VAL

  Permute: seq of VAL -> set of seq of VAL
  RestSeq: seq of VAL * nat1 -> seq of VAL
  PatternIds: Pattern +> set of UniqueId

~~~{% endraw %}

###env.vdmsl

{% raw %}
~~~

types
  ENVL = seq of ENV;
  ENV = seq of BlkEnv;
  BlkEnv = seq of NameVal;
  NameVal = UniqueId * VAL;
  UniqueId = (Name * Position * ([Name * VAL]));
  LVAL = set of (VAL * Model);
  Model = map UniqueId to VAL;
  VAL = NUM | BOOL | SET;
  NUM :: v : int;
  BOOL :: v : bool;
  SET :: v : set of VAL

state Sigma of

operations
  CreateContext: Definitions ==> ()
  InstallValueDefs: seq of ValueDef ==> ()


  InstallFnDefs: map Name to ExplFnDef ==> ()
  InstallCurFn: Name * VAL * set of UniqueId ==> ()
  LeaveCurFn: () ==> ()

operations
  PopEnvL: () ==> ()
  TopEnvL : () ==> ENV
  PushEmptyEnv : () ==> ()
  PopBlkEnv : () ==> ()
  PushBlkEnv : BlkEnv ==> ()
  MkEmptyBlkEnv: () ==> BlkEnv
  CombineBlkEnv : BlkEnv * BlkEnv ==> BlkEnv
  MkBlkEnv : (Name * Position) * VAL ==> BlkEnv
  FnInfo: () ==> [Name * VAL]
  LooseLookUp: Name ==> LVAL
  LookUpValueDefs: Name ==> LVAL
  LookUpFn: Name ==> Pattern * Expr
functions
  SelName: UniqueId +> Name
  SelNameAndPos: UniqueId +> Name * Position
  SelDom: BlkEnv +> set of UniqueId
  Look: BlkEnv * UniqueId +> VAL
  Extend: (map UniqueId to LVAL) * (map UniqueId to LVAL) +>


~~~{% endraw %}

###expr.vdmsl

{% raw %}
~~~


operations
  LooseEvalExpr: Expr ==> LVAL
  LooseEvalBracketedExpr : BracketedExpr ==> LVAL
  LooseEvalLetExpr : LetExpr ==> LVAL
    let val_lv = LooseEvalExpr(expr) in
  LooseEvalLetBeSTExpr : LetBeSTExpr ==> LVAL
    for all mk_(env,m) in set EvalBind(lhs) do 
  LooseEvalIfExpr : IfExpr ==> LVAL
  let test_lv = LooseEvalExpr(test) in
  LooseEvalCasesExpr: CasesExpr ==> LVAL
   let sel_lv = LooseEvalExpr(sel)
  LooseEvalBinaryExpr: BinaryExpr ==> LVAL
  LooseEvalSetBinaryExpr: LVAL * LVAL ==> LVAL
  LooseEvalEqBinaryExpr: LVAL * LVAL ==> LVAL
  LooseEvalNumBinaryExpr: LVAL * BinaryOp * LVAL ==> LVAL
  LooseEvalSetEnumerationExpr: SetEnumerationExpr ==> LVAL
     if len els = 0
           for index = 2 to len els do
  LooseEvalApplyExpr: ApplyExpr ==> LVAL
    let arg_lv = LooseEvalExpr(arg_e),
  LooseEvalLiteral: Literal ==> LVAL



~~~{% endraw %}

###pat.vdmsl

{% raw %}
~~~

operations
  PatternMatch : Pattern * VAL ==> set of BlkEnv

MatchSetEnumPattern : SetEnumPattern * VAL ==> set of BlkEnv
MatchSetUnionPattern : SetUnionPattern * VAL ==> set of BlkEnv
MatchLists : seq of Pattern * seq of VAL ==> set of BlkEnv
UnionMatch : set of BlkEnv ==> set of BlkEnv
StripDoubles : BlkEnv ==> BlkEnv
EvalBind : Bind ==> set of (BlkEnv * Model)
EvalSetBind : SetBind ==> set of (BlkEnv * Model)

~~~{% endraw %}
