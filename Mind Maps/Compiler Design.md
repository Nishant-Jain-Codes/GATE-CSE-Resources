<!-- use this site to create your own mind map -->
<!-- https://markmap.js.org/repl -->
---
markmap:
  initialExpandLevel: 2
---

- [Compiler Design Mind Map by Nishant](https://www.notion.so/GATE-2025-a84ab6d79cf141a4b0b5873933034747?pvs=21)
    - Introduction
        - what is compiler
        - what is translator
        - what is preprocessor and its tasks
        - what are Dynamic Link Libraries
        - phases of compiler ( input and output of each phase )
        - what is linker and its tasks
        - what is loader and its tasks
        - Errors
            - preprocessor errors
            - compiler errors
                - lexical
                - syntax
                - sementic
            - linker errors
            - runtime errors
        - 2 pass compiler architecture and its advantages
    - 1st pass ( High level Code → Intermediate Code ) ( hardware independent ) (4 phases )
        - Lexical Analysis
            - what happens in this phase ? ( character stream → token stream (longest Prefix rule) ) → its checked if valid “words” are given as input
            - symbol table created here . but used in every phase
            - what is symbol table & its structure
            - Types of tokens
                - keywords | ( sizeOf is a keyword and an operator but (exit,main) are not keywords)
                - operators
                - identifiers ( rules of valid identifer )
                - constants (rules of valid constants (ex: number in ocatl can have digit(x) : 0≤ x ≤7 only )
                - special tokens
            - counting / finding tokens from the input string
            - lexical error examples
        - Syntax Analysis
            - what happens in this phase ? ( token stream → parse tree ) → its checked if valid “grammar” (structure) is being followed
            - finding syntax error in code
            - CFG ( context free grammar )
                - CFG = ( Variables , Terminals , Productions , Start symbol ) | production rule of valid CFG | V → (T U V)*
                - used here since it deals with structure only ( i.e no context needed/checked here ) ( CFG vs CSG)
                - Derivation of string
                    - Linear → ( Left most derivation & Right most derivation )
                    - Non - Linear → ( Parse tree )
                - Ambiguity of grammar | Trick : if left and right recursion with same source variable then grammar will be always ambigious
                - logic and formula for elemination of left recursion (direct & indirect) ( since infinite loop when using LMD with grammar having left recursion )
                    
                    ```
                    Original Production: 
                    A → Aα1 | Aα2 |... Aαn | β1 | β2 |... βm 
                    
                    Transformed Productions:
                    A → β1X | β2X |... βmX
                    X → α1X| α2X |... αnX | ε
                    ```
                    
                - left factoring ( removing common left prefix to prevent backtracking problem in top down parser )
                - how to calculate FIRST(…) SET
                - how to calculate FOLLOW(…) SET
            - Parsers ( syntax analyzers )
                - what is parser ?
                - TDP vs BUP
                - structure of parsers ( input string , stack , parsing algorithm , symbol table )
                - Top down parsers (TDPs)
                    - Reads Left to right & follows LMD | start from S and produce the string
                    - LL1 parser ( Left to right read , Left most derivateion , 1 look ahead )
                        - ways to implemnt LL1
                            - recursive | every production will have a recursive function associated with it
                            - tabular | every production will be stored in a table
                        - How to write LL1 CFG | unambigous CFG → non left recursive CFG → left factored CFG → LL1 table → atmost 1 entry in each cell
                        - How to create LL1 Table | compute first set of all V → if null production in any first(x) then compute follow(x) → fill the table using FIRST and FOLLOW sets
                        - How to identify LL1 CFG
                            - theory | unambigous CFG & non left recursive CFG & left factored CFG & LL1 table & atmost 1 entry in each cell
                            - Trick
                                1. X → a | B | c | D .. ( then intersection of first set of every pair of the production should be null )  
                                2. X → a | B | c | null …. (then check 1 & intersection of first set of every production and follow set of X must be null ) 
                        - necessary & sufficient condition for CFG to be LL1 ( each cell in LL1 table have atmost 1 entry )
                        - LL parsing algorithm
                            1. if `st.top() == V` , then `st.pop()` , and `push (Table[V,input[ptr]]`) in stack in reverse order 
                            2. if `st.top() == T` , then `st.pop()`, and `ptr++`
                        - Conflits in LL1 parsing
                            - N ( non terminal ) conflict | `st.top() == V && table[V,input[ptr]] == NULL`
                            - T ( terminal ) conflict | `st.top() == T && st.top() ≠ input[ptr]`
                    - LL(k) parsers and properties
                        - k tells the lookahed i.e length of common prefix which algo can diffentiate , ex k == 2 then it will be able to diffrentiate between ab & ac .
                        - every LL(K) CFG is LL(K+1)
                        - every LL(K) CFG can be converted to LL(1) cfg
                    - for every DCFL (deterministic context free language) , LL1 CFG exists and every LL1 CFG produces a DCFL
                - Bottom up parsers (BUPs)
                    - Reads Left to right & follows RMD in Reverse  | start from input string and come to S
                    - different parsers
                        - LR0 ( Left to right read , RMD in reverse , 0 look ahead )
                        - SLR ( Simple , Left to Right read , RMD in reverse )
                        - CLR ( Canonical i.e. exact , Left to Right read , RMD in reverse ) also called LR(1)
                        - LALR (Look ahead , LR (1) )
                    - DFA construction for different BUPs
                        - shift , reduce , accept items
                        - Calculating Closure
                        - Calculating GOTO
                        - Calculating Look Ahead ( only for CLR and LALR )
                        - Note : DFA for LR0 & SLR & LALR are same ( but LALR also have lookahead set along with the productions )
                        - Note : after joining common states of CLR’s DFA we get LALR’s DFA
                    - Conflicts in differnet BUP
                        - Shift Reduce Confict (SR)
                            - LR0 → if both shift and reduce item in same state
                            - SLR → if shift terminal belong to follow set of reduced production
                            - CLR , LALR→ if shift terminal belong to look ahead set of reduced production
                        - Reduce Reduce Confict (RR)
                            - LR0 → if ≥2 Reduce item in a state
                            - SLR → if intersection of follow sets of reduced production is not empty
                            - CLR , LALR→ if intersectino of look ahead set of reduced productions is not empty
                    - what error is possible & impossible  when CLR’s states are merged to create LALR respectively ? → RR & SR
                    - Table consruction
                        - action table
                            - shift entries
                            - accept entries
                            - reduce entries ( different for LR0 , SLR , LALR , CLR )
                        - Goto table → goto entry
                    - LR parsing Algorithm
                - Relations among different ( parsers , grammars , classes of language generated by grammars )
                    - Paresers Power
                        - LR0 < SLR < LALR < CLR
                        - LL1 < CLR
                    - Grammars (one way implications)
                        - LR0 → SLR → LALR → CLR → Unambigious
                        - LL1 → CLR → Unambigious
                    - Language Classes ( Set of language produced by corresponding grammars ) Language(G)
                        - Language(LR0) ⊂ Language(SLR) ⊂ Language(LALR) ⊂ Language(CLR) ⊂ Language(Unambigious grammar)
                        - Language(LL1) ⊂ Language(CLR) ⊂ Language(Unambigious grammar)
            - Precedence Relations
                - what is precedence relation → tells us order of resolving operator evaluations
                - how to calculate prioirty & associativity of operators → ( use precidence table ) | ( trick : left / right recursion with operator then that operator is left / right associative , respectively )
                - what is operator grammar → no null production and no 2 non-terminal together
            - CYK algo → member ship algo of CFG ( tells if string w belongs to L(G) , where G is CFG ) → time complexity O(n^3)
        - Syntax Directed Translation ( also used for sementic analysis )
            - Syntax ( CFG i.e. grammar ) + Directed ( since order of productions and translation matters ) + Translation ( giving meaning to production rules , ex A→B#C means A = B+C or B*C )
            - application of SDTs
            - lexitcal Vs Syntax Vs Sementic Vs SDT
            - Type of Attributes ( Inherited (derived from sibling or parent) , Synthesized (derived from child))
            - Type of SDTs ( L - attributed , S - attributed ) → depends on location of Translation and type of attributes
            - Evaluation of SDTs ( Left to right ( depends on Translation order ) , Bottom up parsing ( reverse of RMD ) , Top down parsing ( LMD) , NOTE : BUP and TDP evalutaions , doesnt depend on translation order )
            - if givent SDTs is ambigious it’s possible we get multiple output at root for the same input , thus we have to check for that too and select answer accordingly
        - Intermediate Code Generation
            - Intermediate Code representation ( postfix , Three Adress Code , Static Single Assignment , Syntax Tree , Directed Acyclic Graph , Control Flow Graph )
            - Three Adress Code Notation ( triple , quadruple , indirect triple )
            - Three Adress Code for ( if - else , while loop , accessing 2d array element )
            - Control Flow Graph → consists of nodes & edges & Basic Blocks ( control must enter and exit the block at the 1st and last stement respetively & the control must not exit / enter the block in / from middle )
    - 2nd pass ( Intermediate Code → Target Code ) ( hardware dependent ) (3 phases)
        - Intermediate Code optimisation
            - optimization techniques
                - constant folding
                - copy propogation → constant & variable
                - common subexpression elemination
                - strength reduction
                - algebraic simplifications
                - dead code elemination
                - loop optimisation → code motion , induction variable elemniation , loop mergin , loop unrolling
            - Data Flow Analysis
                - Live variable analysis
                    - what is a live variable ?
                    - calculating live variable at a statement Si
                - Backward analysis to obtain live variable at start and end of every Basic Block in the code
                    - KILL , GEN , IN , OUT sets calculation
                    - INk = (OUTk - KILLk) U GENk
                    - OUTk = union of all the INset of its next Basic blocks
                - Reaching Definition
                    - USE-DEF chain ( where read → where write )
                    - DEF-USE chain ( where write → where read )
                - Dominator Sequence , Tree , elements
        - Aseembly Code Generation ( Intermediate code → assembly Code )
            - hardware dependent , must have knowledge of H/w architecture
            - register allocation using graph colouring algorithm
        - Aseembly Code optimization
    - Runtime environment
        - parameter passing ( by value & by reference )
        - memory alloation ( static , dynamic ( stack , heap ) )
        - activation record