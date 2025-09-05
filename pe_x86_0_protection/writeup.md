# Step 1 – Analysis with Ghidra

We open the PE X86 binary in Ghidra.
We quickly find the function FUN_00401726(char *param_1, int param_2), which handles password verification.

# Step 2 – Disassembly

The function performs a sequence of comparisons:

00401733 83 7d 0c 07     CMP        dword ptr [EBP + param_2],0x7
00401737 75 71           JNZ        LAB_004017aa
00401739 8b 45 08        MOV        EAX,dword ptr [EBP + param_1]
0040173c 0f b6 00        MOVZX      EAX,byte ptr [EAX]
0040173f 3c 53           CMP        AL,0x53
00401741 75 67           JNZ        LAB_004017aa
00401743 8b 45 08        MOV        EAX,dword ptr [EBP + param_1]
00401746 83 c0 01        ADD        EAX,0x1
00401749 0f b6 00        MOVZX      EAX,byte ptr [EAX]
0040174c 3c 50           CMP        AL,0x50
0040174e 75 5a           JNZ        LAB_004017aa
00401750 8b 45 08        MOV        EAX,dword ptr [EBP + param_1]
00401753 83 c0 02        ADD        EAX,0x2
00401756 0f b6 00        MOVZX      EAX,byte ptr [EAX]
00401759 3c 61           CMP        AL,0x61
0040175b 75 4d           JNZ        LAB_004017aa
0040175d 8b 45 08        MOV        EAX,dword ptr [EBP + param_1]
00401760 83 c0 03        ADD        EAX,0x3
00401763 0f b6 00        MOVZX      EAX,byte ptr [EAX]
00401766 3c 43           CMP        AL,0x43
00401768 75 40           JNZ        LAB_004017aa
0040176a 8b 45 08        MOV        EAX,dword ptr [EBP + param_1]
0040176d 83 c0 04        ADD        EAX,0x4
00401770 0f b6 00        MOVZX      EAX,byte ptr [EAX]
00401773 3c 49           CMP        AL,0x49
00401775 75 33           JNZ        LAB_004017aa
00401777 8b 45 08        MOV        EAX,dword ptr [EBP + param_1]
0040177a 83 c0 05        ADD        EAX,0x5
0040177d 0f b6 00        MOVZX      EAX,byte ptr [EAX]
00401780 3c 6f           CMP        AL,0x6f
00401782 75 26           JNZ        LAB_004017aa
00401784 8b 45 08        MOV        EAX,dword ptr [EBP + param_1]
00401787 83 c0 06        ADD        EAX,0x6
0040178a 0f b6 00        MOVZX      EAX,byte ptr [EAX]
0040178d 3c 53           CMP        AL,0x53
0040178f 75 19           JNZ        LAB_004017aa
00401791 b8 53 40 40 00  MOV        EAX,s_Gratz_man_:)_00404053

At the end if all comparisons succeed the string "Gratz man :)" is loaded.

# Step 3 – Decompiled function

In Ghidra this function looks like:

void __cdecl FUN_00401726(char *param_1,int param_2)
{
  if (((((param_2 == 7) && (*param_1 == 'S')) && (param_1[1] == 'P')) &&
      ((param_1[2] == 'a' && (param_1[3] == 'C')))) &&
     ((param_1[4] == 'I' && ((param_1[5] == 'o' && (param_1[6] == 'S')))))) {
    printf("Gratz man :)");
    exit(0);
  }
  puts("Wrong password");
  return;
}

# Step 5 – Result

Correct password → outputs:
Gratz man :)

Wrong password outputs:
Wrong password