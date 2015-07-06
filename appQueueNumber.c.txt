// ---------------------------------------------------------------------------
// $NetworkLink: http://www.oschina.net/code/snippet_1865396_36953 $
// $Workfile: appQueueNumber.c $
// $Revision: 1 $
// $Author: linwenfu8910@163.com $
// $Date: 14-07-03 9:47 $
// $Related: http://www.oschina.net/code/snippet_1771722_35779?p=2#comments
// ---------------------------------------------------------------------------
#include <stdio.h>
 
//#define __DEBUG__
//#define __CHECK_EACH_SPECIAL_NUMBER__
//#define __CALCULATE_BY_NUMBUFFER__
 
typedef unsigned char BOOL;
 
enum
{
    FALSE,
    TRUE,
};
 
typedef enum
{
    ecwFIZZ,
    ecwBUZZ,
    ecwWHIZZ,
    ecwINVALID,
}eCODEWORD;
 
#define MAX_QUEUE_NUM 100       // MAX Queue number
#define MAX_QUEUE_CYCLE 3       // Current Queue max number item (100 means 3, 10 means 2)
#define MAX_INPUT_NUM ecwINVALID    // MAX input number 
 
static const char * sm_cCodeWord[MAX_INPUT_NUM]=
{
    "Fizz",
    "Buzz",
    "Whizz",
};
 
//--------------------------------------------------------------------------
// Function define
//--------------------------------------------------------------------------
static void si_fnPrintProgramInfo(void);
static void si_fnPrintCodeWord(int nCodeWord);
static void si_fnPrintCurrentNum(int nNum);
static void si_fnInputCodeWordNumber(int * pnInputNum, int nLength);
static  BOOL si_fnCheckCodeWord(int nCurrentNum, int nCheckNum, BOOL bSpecial);
static int si_fnPowNumber(int nBase, int nPow);
static void si_fnQueueNumber(int *pnInputNum, int nInputLength);
 
 
/*************************************************************************/
/*!
   @FuncName
            si_fnPrintProgramInfo
   @Function
            Print current program's information and purpose
   @Argument
            none
   @Return
            none
   @Detail
            none
**************************************************************************/
static void si_fnPrintProgramInfo(void)
{
    int nIndex = 0;
    printf("This program use for Queue Number (Max number is %d)\n", MAX_QUEUE_NUM);
    printf("PLease input %d numbers, the code word is \n", MAX_INPUT_NUM);
    for(nIndex = 0; nIndex < MAX_INPUT_NUM; nIndex++)
    {
        printf("%d : %s\t", nIndex+1, sm_cCodeWord[nIndex]);
    }
    printf("\n");
    printf("Rule 1: If Current number have the first input number then print its Code Word\n");
    printf("Rule 2: If Current number can devide the input number then print its Code Word\n");
    printf("Rule 3: If Current number can devide the input 2 numbers then print the 2 Code Words \n");
    printf("Rule 4: If Current number can devide the input 3 numbers then print the 3 Code Words \n");
    printf("So right now, if you copy me, then enter in 3 Number and enjoy yourself \n");
}
 
/*************************************************************************/
/*!
   @FuncName
            si_fnPrintCodeWord
   @Function
            Print the Code word
   @Argument
            nCodeWord : code word property
   @Return
            none
   @Detail
            none
**************************************************************************/
static void si_fnPrintCodeWord(int nCodeWord)
{
    eCODEWORD eCodeWord = 0;
     
    while(nCodeWord != 0 && eCodeWord<ecwINVALID)
    {
        if(nCodeWord & 0x01)
        {
            printf("%s", sm_cCodeWord[eCodeWord]);
        }
        eCodeWord++;
        nCodeWord = nCodeWord>>1;
    }   
    printf("\n");
}
 
/*************************************************************************/
/*!
   @FuncName
            si_fnPrintCurrentNum
   @Function
            Print the Code word
   @Argument
            nNum : number to print
   @Return
            none
   @Detail
            none
**************************************************************************/
static void si_fnPrintCurrentNum(int nNum)
{
    printf("%d\n", nNum);
}
 
/*************************************************************************/
/*!
   @FuncName
            si_fnPrintInputCodeWordInfo
   @Function
            Print Uer's confirm information 
   @Argument
            pnInputNum : input number buffer
            nLength : input number length 
   @Return
            none
   @Detail
            none
**************************************************************************/
static void si_fnPrintInputCodeWordInfo(int *pnInputNum, int nLength)
{
    int nIndex = 0;
    printf("Confirm the information \n");
    for(nIndex = 0; nIndex < nLength; nIndex++)
    {
        printf("%d : %s\t", pnInputNum[nIndex], sm_cCodeWord[nIndex]);
    }
    printf("\n");
}
 
/*************************************************************************/
/*!
   @FuncName
            si_fnInputCodeWordNumber
   @Function
            Get User's input number 
   @Argument
            pnInputNum : input number buffer
            nLength : input number length 
   @Return
            none
   @Detail
            Make sure that the input number is between 1~9
            Each 2 number can't be the same
**************************************************************************/
static void si_fnInputCodeWordNumber(int * pnInputNum, int nLength)
{
    int nCount = 0;
    int nIndex = 0;
    BOOL bEqual = FALSE;
    BOOL bContinue = TRUE;
    do
    {
        for(nIndex = 0; nIndex < nLength; nIndex++)
        {
            printf("Enter in the %d number : ", nIndex+1);
            scanf("%d", &pnInputNum[nIndex]);
        }
        bContinue = FALSE;
        bEqual = FALSE;
        for(nIndex = 0; nIndex < nLength; nIndex++)
        {
            if(0 >= pnInputNum[nIndex] || 9 < pnInputNum[nIndex])
            {
                bContinue = TRUE;
                printf("Please enter in the number 1~9 \n");
                printf("Please re-enter again !\n");
                break;
            }
            else
            {
                for(nCount = nIndex+1; nCount<nLength; nCount++)
                {
                     
                    if(pnInputNum[nCount] == pnInputNum[nIndex])
                    {
                        bEqual = TRUE;
                        break;
                    }
                }
                 
                if(bEqual)
                {
                    bContinue = TRUE;
                    printf("Please do not enter in the equal number \n");
                    printf("Please re-enter again !\n");
                    break;
                }
            }
             
        }
    }while(bContinue);
}
/*************************************************************************/
/*!
   @FuncName
            si_fnCheckCodeWord
   @Function
            Check Current number whether can convert to Code word or not.
   @Argument
            nCurrentNum : current number to check 
            nCheckNum : input check number
            bSpecial : input number is special number or not
   @Return
            TRUE/FALSE
   @Detail
            NONE
**************************************************************************/
static  BOOL si_fnCheckCodeWord(int nCurrentNum, int nCheckNum, BOOL bSpecial)
{
    BOOL bStatus = FALSE;
    if(bSpecial)
    {
        if(nCurrentNum == nCheckNum)
        {
            bStatus = TRUE;
        }
    }
    else
    {
        if((nCurrentNum % nCheckNum) == 0 )
        {
            bStatus = TRUE;
        }   
    }
    return bStatus;
}
 
/*************************************************************************/
/*!
   @FuncName
            si_fnPowNumber
   @Function
            calculate the pow
   @Argument
            nBase : base number 
            nPow : pow number
   @Return
            nBase*...*nBase (npow item)
   @Detail
            NONE
**************************************************************************/
static int si_fnPowNumber(int nBase, int nPow)
{
    int nResult = 0;
    if(nPow == 0)
    {
        nResult = 1;
    }
    else if(nPow == 1)
    {
        nResult = nBase;
    }
    else
    {
        nResult = nBase * si_fnPowNumber(nBase, nPow - 1);
    }
    return nResult;
}
 
/*************************************************************************/
/*!
   @FuncName
            si_fnQueueNumber
   @Function
            Queue the number
   @Argument
            pnInputNum : input number buffer
            nLength : input number length 
   @Return
            NONE
   @Detail
            See the Program information for detail. main logic
**************************************************************************/
static void si_fnQueueNumber(int *pnInputNum, int nInputLength)
{
    int nCheckResult = 0;
    int nIndex = 0;
    int nCheckLength = 0;
    int nCheckNum = 0;
    int nCheckNumBuffer[MAX_QUEUE_CYCLE] = {0}; 
    printf(">>>>>>>>>>>>> Begin to Queue the number right now <<<<<<<<<<<<<< \n");
    do
    {
        nCheckNumBuffer[MAX_QUEUE_CYCLE -1]++;
     
        if(MAX_QUEUE_CYCLE >= 2)
        {
            for(nIndex = 0; nIndex < MAX_QUEUE_CYCLE - 1; nIndex++)
            {
                if(nCheckNumBuffer[MAX_QUEUE_CYCLE -1 - nIndex] == 10)
                {
                    nCheckNumBuffer[MAX_QUEUE_CYCLE -1 - nIndex] = 0;
                    nCheckNumBuffer[MAX_QUEUE_CYCLE - 2 - nIndex]++;
                }   
            }
        }
 
        // Check the Special number, in this rule, just check the first number
        for(nIndex = 0; nIndex < MAX_QUEUE_CYCLE; nIndex++)
        {
            nCheckResult = 0;
        #if defined(__CHECK_EACH_SPECIAL_NUMBER__)
            // If you want to check each number, then use the loop
            for(nCheckLength = 0; nCheckLength<nInputLength; nCheckLength++)
            {
                nCheckResult |= si_fnCheckCodeWord(nCheckNumBuffer[nIndex], pnInputNum[nCheckLength], TRUE) << nCheckLength;
                if(nCheckResult != 0)
                {
                    break;
                }
            }
            if(nCheckResult != 0)
            {
                break;
            }
        #else // Just check firt number
            nCheckLength = 0;
            nCheckResult |= si_fnCheckCodeWord(nCheckNumBuffer[nIndex], pnInputNum[nCheckLength], TRUE) << nCheckLength;
            if(nCheckResult != 0)
            {
                break;
            }
        #endif//#if 1
             
        }
     
    #if defined(__CALCULATE_BY_NUMBUFFER__)
        // Calculate current value from nCheckNumBuffer
        nCheckNum = 0;
        for(nIndex = 0; nIndex < MAX_QUEUE_CYCLE; nIndex++)
        {
            nCheckNum += nCheckNumBuffer[nIndex] * si_fnPowNumber(10, MAX_QUEUE_CYCLE - nIndex - 1);
        }
    #else
        nCheckNum++;
    #endif //#if defined(__CALCULATE_BY_NUMBUFFER__)
    #if defined(__DEBUG__)
        printf(" %d%d%d, %d ", nCheckNumBuffer[0], nCheckNumBuffer[1], nCheckNumBuffer[2], nCheckNum);
    #endif //#if defined(__DEBUG__)
        if(nCheckResult == 0)
        {
            for(nCheckLength = 0; nCheckLength<nInputLength; nCheckLength++)
            {
                nCheckResult |= si_fnCheckCodeWord(nCheckNum, pnInputNum[nCheckLength], FALSE) << nCheckLength;
            }
        }
         
        if(nCheckResult == 0)
        {
            si_fnPrintCurrentNum(nCheckNum);
        }
        else
        {
        #if defined(__DEBUG__)
            printf("nCheckNum = %d, nCheckResult = %d ", nCheckNum, nCheckResult);
        #endif //#if defined(__DEBUG__)
            si_fnPrintCodeWord(nCheckResult);
        }        
    }while(nCheckNum<MAX_QUEUE_NUM);
 
    printf(">>>>>>>>>>>>> End Queue <<<<<<<<<<<\n");
}
 
void main(void)
{
    int nInputNum[MAX_INPUT_NUM] = {0}; 
    si_fnPrintProgramInfo();
    si_fnInputCodeWordNumber(nInputNum, MAX_INPUT_NUM);
    si_fnPrintInputCodeWordInfo(nInputNum, MAX_INPUT_NUM);
    si_fnQueueNumber(nInputNum, MAX_INPUT_NUM);
    return ;
}