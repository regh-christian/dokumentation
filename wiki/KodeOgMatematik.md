# Kode og Matematik
## TEST KODE

Tekst før kodeblok.
```
import java.util.*;
public class LowestCommonMultiple {
  
    private static long
    lcmNaive(long numberOne, long numberTwo)
    {
  
        long lowestCommonMultiple;
  
        lowestCommonMultiple
            = (numberOne * numberTwo)
              / greatestCommonDivisor(numberOne,
                                      numberTwo);
  
        return lowestCommonMultiple;
    }
  
    private static long
    greatestCommonDivisor(long numberOne, long numberTwo)
    {
  
        if (numberTwo == 0)
            return numberOne;
  
        return greatestCommonDivisor(numberTwo,
                                     numberOne % numberTwo);
    }
    public static void main(String args[])
    {
  
        Scanner scanner = new Scanner(System.in);
        System.out.println("Enter the inputs");
        long numberOne = scanner.nextInt();
        long numberTwo = scanner.nextInt();
  
        System.out.println(lcmNaive(numberOne, numberTwo));
    }
}

```
Tekst efter kodeblok.

## Matematik
This sentence uses `$$` delimiters to show math inline:  $$\sqrt{3x-1}+(1+x)^2$$

**The Cauchy-Schwarz Inequality**

$$\left( \sum_{k=1}^n a_k b_k \right)^2 \leq \left( \sum_{k=1}^n a_k^2 \right) \left( \sum_{k=1}^n b_k^2 \right)$$
