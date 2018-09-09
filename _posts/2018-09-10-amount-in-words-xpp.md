---
layout: post
title: "Get amount in words with decimal precision using x++"
date: 2018-09-10 12:00:00 +0500
categories: ax-dynamics x++
tags: amount ax-dynamics x++
comments: 1
---

To convert any given real number to amount in words with decimal precision, use following code snippet.

{% highlight cs %}
/// <summary>
/// Converts numerical amount to amount in words
/// </summary>
/// <param name = "_amount">Amount in numeric format</param>
/// <param name = "_noOfDecimals">number of decimals upto which amount in words should be returned</param>
/// <returns>Amount in string format upto defined decimal precision</returns>

public str getAmountInWords(real _amount, int _noOfDecimals)
{
    int     amount;
    str     amount2Words, decimalPlaces2Words;
    int     NO_OF_DECIMAL = (_noOfDecimals < 0) ? 0 : real2int(power(10, _noOfDecimals));
    
    amount       = real2int(_amount);
    amount2Words = Global::numeralsToTxt(amount);
    amount2Words = subStr(amount2Words, 5, strLen(amount2Words) - 4);
    amount2Words = subStr(amount2Words, strLen(amount2Words) - 10, -strLen(amount2Words));

    // digits after decimal
    amount = real2int(NO_OF_DECIMAL * (_amount - amount));

    // ignore if there are only zeroes after decimal
    if (amount != 0)
    {
        decimalPlaces2Words = Global::numeralsToTxt(amount);
        decimalPlaces2Words = subStr(decimalPlaces2Words, 5, strLen(decimalPlaces2Words) - 4);
        decimalPlaces2Words = subStr(decimalPlaces2Words, 
                                     strLen(decimalPlaces2Words) - 10, 
                                     -strLen(decimalPlaces2Words));
        amount2Words += strFmt("and %1", decimalPlaces2Words);
    }

    amount2Words = strFmt("%1 only", amount2Words);
    amount2Words = str2CapitalWord(amount2Words);

    return amount2Words;
}
{% endhighlight %}