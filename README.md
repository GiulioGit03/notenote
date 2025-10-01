using System.Reflection.Metadata.Ecma335;
 
namespace Basic_DeNico
{
    internal class Program
    {
        static void Main(string[] args)
        {
          Console.WriteLine(  DeNico("crazy", "cseerntiofarmit on  "));
        }
        public static string DeNico(string key, string message)
        {
            string sortedword = new string(key.OrderBy(c => c).ToArray());
            int[] positions = new int[sortedword.Length];
            for (int i = 0; i < key.Length; i++)
            {
                for (int j = 0; j < sortedword.Length; j++)
                {
                    if (key[i] == sortedword[j])
                    {
                        positions[i] = j + 1;
                    }
 
                }
            }
            char charToRemove = ' ';
            string messageResult = message.Replace(charToRemove.ToString(), "");
            Console.WriteLine(messageResult);
            int run = 0;
            string finalmessage = "";
 
            while (run < ((messageResult.Length % key.Length) + 1))
            {
                for (int i = 0; i < key.Length; i++)
                {
                    int chiave = positions[i];
                    finalmessage += messageResult[chiave];
                }
                run++;
            }
                return finalmessage;
 
            }
        }
 
 
 
    }
