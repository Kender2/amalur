Kore VM is based on LUA version 5.1 and is supposedly compatible with that version.

All the scripts in the game are compiled to bytecode with debug information stripped.

Intro into lua internals https://www.lua.org/doc/jucs05.pdf

#### Header

The header of these is slightly different from standard luac files:

    1b 4c 75 61 51 08 00 04 04 04 04 01 01
    where
    1b 4c 75 61 = magic bytes
    51 = Compatible with Lua 5.1
    08 = Format version (00 = plain Lua, I guess 08 is Havik Script format)
    00 = Big endian (01 = little endian)
    04 = integers are 4 bytes long
    04 = size_t is 4 bytes long
    04 = Instrcutions are 4 bytes long
    04 = Lua numbers are 4 bytes long (standard is 8)
    01 = Integral numbers are used
    01 = Bytecode Sharing Mode? (not in standard lua)

More information on lua format: http://luaforge.net/docman/83/98/

#### Functions

Lua can be extended with C code, which is what the developers used to add tons of new functions, ready to be used from scripts.
These are listed in the functions.txt file.

See https://www.lua.org/pil/24.html for more information on the topic.

#### Opcodes

The instruction set of the bytecode is greatly expanded. 86 opcodes vs lua's original 38.

Byte|Mode|A|B|C|?|?|?|Opcode
----|----|-|-|-|-|-|-|------
0|0|1|3|5|1|0|43|GETFIELD
1|0|1|0|1|0|0|41|TEST
2|0|1|1|1|0|0|3F|CALL_I
3|0|1|1|1|0|0|56|CALL_C
4|0|0|4|4|0|0|56|EQ
5|0|0|4|4|0|0|56|EQ_BK
6|1|1|5|1|1|0|56|GETGLOBAL
7|0|1|3|0|1|0|56|MOVE
8|0|1|3|4|1|0|56|SELF
9|0|1|1|0|0|0|56|RETURN
0A|0|1|3|4|0|0|56|GETTABLE_S
0B|0|1|3|4|0|0|56|GETTABLE_N
0C|0|1|3|4|0|0|56|GETTABLE
0D|0|1|1|1|0|0|56|LOADBOOL
0E|0|1|0|1|0|0|56|TFORLOOP
0F|0|1|5|4|0|0|44|SETFIELD
10|0|1|4|4|0|0|56|SETTABLE_S
11|0|1|4|4|0|0|56|SETTABLE_S_BK
12|0|1|4|4|0|0|56|SETTABLE_N
13|0|1|4|4|0|0|56|SETTABLE_N_BK
14|0|1|4|4|0|0|56|SETTABLE
15|0|1|4|4|0|0|56|SETTABLE_BK
16|0|1|1|1|0|0|3E|TAILCALL_I
17|0|1|1|1|0|0|56|TAILCALL_C
18|0|1|1|1|0|0|56|TAILCALL_M
19|1|1|5|0|0|0|56|LOADK
1A|0|1|3|0|0|0|56|LOADNIL
1B|1|1|5|0|0|0|56|SETGLOBAL
1C|2|0|2|0|0|0|56|JMP
1D|0|1|1|1|0|0|56|CALL_M
1E|0|1|1|1|0|0|56|CALL
1F|0|1|1|1|0|0|56|TAILCALL
20|0|1|1|0|1|0|56|GETUPVAL
21|0|1|1|0|0|0|40|SETUPVAL
22|0|1|4|4|0|0|56|ADD
23|0|1|4|4|0|0|56|ADD_BK
24|0|1|4|4|0|0|56|SUB
25|0|1|4|4|0|0|56|SUB_BK
26|0|1|4|4|0|0|56|MUL
27|0|1|4|4|0|0|56|MUL_BK
28|0|1|4|4|0|0|56|DIV
29|0|1|4|4|0|0|56|DIV_BK
2A|0|1|4|4|0|0|56|MOD
2B|0|1|4|4|0|0|56|MOD_BK
2C|0|1|4|4|0|0|56|POW
2D|0|1|4|4|0|0|56|POW_BK
2E|0|1|1|1|1|0|56|NEWTABLE
2F|0|1|3|0|0|0|56|UNM
30|0|1|3|0|0|0|42|NOT
31|0|1|3|0|0|0|56|LEN
32|0|0|4|4|0|0|56|LT
33|0|0|4|4|0|0|56|LT_BK
34|0|0|4|4|0|0|56|LE
35|0|0|4|4|0|0|56|LE_BK
36|0|1|1|1|0|0|56|CONCAT
37|0|1|3|1|0|0|56|TESTSET
38|2|1|2|0|0|0|56|FORPREP
39|2|1|2|0|0|0|56|FORLOOP
3A|0|1|1|2|0|0|56|SETLIST
3B|0|1|0|0|0|0|56|CLOSE
3C|1|1|1|0|0|0|56|CLOSURE
3D|0|1|1|0|0|0|56|VARARG
3E|0|0|1|1|0|1|56|TAILCALL_I_R1
3F|0|0|1|1|0|1|56|CALL_I_R1
40|0|1|1|0|1|1|56|SETUPVAL_R1
41|0|1|0|1|0|1|56|TEST_R1
42|0|1|3|0|0|2|56|NOT_R1
43|0|1|3|5|1|2|56|GETFIELD_R1
44|0|1|5|4|0|1|56|SETFIELD_R1
45|0|1|1|1|1|0|56|NEWSTRUCT
46|1|0|2|0|0|0|56|DATA
47|0|1|0|1|0|0|56|SETSLOTN
48|0|1|1|4|0|0|56|SETSLOTI
49|0|1|1|4|0|0|56|SETSLOT
4A|0|1|1|3|0|0|56|SETSLOTS
4B|0|1|1|4|0|0|56|SETSLOTMT
4C|1|1|1|0|0|0|56|CHECKTYPE
4D|1|1|1|0|0|0|56|CHECKTYPES
4E|0|1|3|1|0|0|56|GETSLOT
4F|0|1|3|1|0|0|56|GETSLOTMT
50|0|1|3|1|0|0|56|SELFSLOT
51|0|1|3|1|0|0|56|SELFSLOTMT
52|0|1|3|5|1|0|56|GETFIELD_MM
53|1|1|1|0|0|0|56|CHECKTYPE_D
54|0|1|3|1|0|0|56|GETSLOT_D
55|1|1|5|1|1|0|56|GETGLOBAL_MEM
56|0|0|0|0|0|0|56|OPCODE_MAX


#### Lua state

In Lua, the lua_State is very important. It gets passed to almost every function.

In regular Lua the state looks like this:
```C
struct lua_State {
  GCObject *next; 
  lu_byte tt; 
  lu_byte marked;
  lu_byte status;
  StkId top;  /* first free slot in the stack */
  StkId base;  /* base of current function */
  global_State *l_G;
  CallInfo *ci;  /* call info for current function */
  const Instruction *savedpc;  /* `savedpc' of current function */
  StkId stack_last;  /* last free slot in the stack */
  StkId stack;  /* stack base */
  CallInfo *end_ci;  /* points after end of ci array*/
  CallInfo *base_ci;  /* array of CallInfo's */
  int stacksize;
  int size_ci;  /* size of array `base_ci' */
  unsigned short nCcalls;  /* number of nested C calls */
  unsigned short baseCcalls;  /* nested C calls when resuming coroutine */
  lu_byte hookmask;
  lu_byte allowhook;
  int basehookcount;
  int hookcount;
  lua_Hook hook;
  TValue l_gt;  /* table of globals */
  TValue env;  /* temporary place for environments */
  GCObject *openupval;  /* list of open upvalues in this stack */
  GCObject *gclist;
  struct lua_longjmp *errorJmp;  /* current error recover point */
  ptrdiff_t errfunc;  /* current error handling function (stack index) */
};
```

In kore vm this apparently works a bit differently. This is what I've got so far.
```C
struct lua_State
{
  lu_byte tt_maybe;
  lu_byte marked_maybe;
  lu_byte status_maybe;
  char unknown_byte;
  int *unknown0;
  global_State *l_G;
  int *unknown2;
  int *stack_last;
  int *base;
  int unknown3;
  int unknown4;
  int unknown5;
  int *top;
  int *stack;
  int *unknown6;
  int *unknown7;
  int unknown8;
  int l_gt;
  int *unknown10;
  int env_maybe;
  int unknown12;
  int unknown13;
  int unknown14;
  int unknown15;
  int *GC_maybe;
};
```
