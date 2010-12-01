!SLIDE small

# Readability and Maintainabiality

!SLIDE center smbullets small
# Guidelines, Not Laws #
* Life is easier if we are all on the same page but there are always exceptions.

!SLIDE small smbullets
# Size And Complexity #
* If a method doesn't fit on your screen completely, it's too long.
* If it's so complex that it needs to be tested separately, it should be an separate class.
* High cyclomatic complexity is a sign you should refactor.
* If an explanation of the method is longer than the method, refactor.

!SLIDE small smbullets
# Complexity Rule Of Thumb #
* You should be able to describe the responsibility of a class in one sentence without a conjunction.
* <span style="color: red">Bad:</span>  This class is responsible for loading a user's data and displaying a user access report .
* <span style="color: green">Good:</span>  This class is responsible for loading a user's data.
<br /><br /><br />
* You should be able to describe the action a method performs without a conjunction.
* <span style="color: red">Bad:</span>  This method verifies the user's password, and loads her profile.
* <span style="color: green">Good:</span>  This method verifies the user's password.  This method loads the user's profile.


!SLIDE small smbullets
# Commenting Complexity #
* Empty xml comments are worse than no comments.
* Use block comments that are meaningful to what is happening.  We store change data in SVN.
* Don't preserve old code.  We store that in SVN too.
* If a brand new hire couldn't tell what your code is doing with one read through it needs more comments, better naming, or less magic.


!SLIDE small smbullets
# Doing One Thing Well #
* A well designed and documented class will be easy to understand and have a single goal.
* Side effects mean it's doing more than one thing
* Side effects shouldn't exist at the data service level; they may exist at the business logic level.
* Isolation should just "happen" and testing shouldn't be "hard"


!SLIDE small smbullets
# Clean Up After Yourself #
## Your mother doesn't work here to do it for you. ##
<br /><br />
* Delete unused code.  You can get it back from SVN if you ever need to.
* Remove comments that don't describe functionality.  Nobody needs to see ancient method sigs.
* Remove unused files from the solution and SVN.  Be heartless to orphaned code.
* When you are done writing code, review and refactor it.  All code can use extra love.
* If you are modifying old code spend a few minutes to clean it up before you commit.

!SLIDE code smaller
# Naming #

The easiest thing to do is to standarize on what the IDE will do automatically.

!SLIDE code smaller
# Logic That Can Be Refactored #
    @@@csharp
    public void ProcessOrder(Order order)
    {
      double minOrderValue = Convert.ToDouble(
          ConfigurationManager.AppSettings["MinOrderValue"]);

      ...

      if (order.Total < minOrderValue)
      {
        order.IsValid = false;
      }

      ...

      FinishProcessingOrder(order);
    }


!SLIDE code smaller
# Logic Refactored #
    @@@csharp
    public void ProcessOrder(Order order)
    {
      var config = LoadConfiguration();

      ValidateOrder(order, config);

      if (order.IsValid)
        FinishProcessingOrder(order, config);
    }

    private OrderProcessingConfig LoadConfiguration()
    {
      return new OrderProcessingConfig
        {
          MinOrderValue = ...
        };
    }
    ...

!SLIDE center smbullets small
# Clever Is Not The Same As Good #
* Clever solutions to difficult problems are good.
* Clever code for its own sake is bad.  Readability and maintainability are more important than you saving 10 keystrokes.
* If you want to use new technology (and you do), SHARE IT with the team.


!SLIDE code smaller
# Clever And Not Good
###We don't write Perl for a reason*.  We don't abuse ? and ?? for the same reason.
    @@@csharp
    // Try to quickly figure out what these two results are
    void Main()
    {
      var w = "Test1";
      var x = 100;
      var y = 50;
      var z = "50";

      // This will result in defenestration
      var result = (x > y) ? (x.ToString() == z) ? 1 : 2 : 3;

      // This will result in worse
      var result2 = (x > y) ? (x.ToString() == z)
        ? 1.ToString() : z ?? w : 3.ToString();
    }

*Not all Perl is hideous.  In theory.


!SLIDE small smbullets
# Clever And Not Good #
* Previous values
* result = 2
* result2 = "50"
* Sparing use of the null-coalescing operator (`??`) and conditional operator (`?:`) are acceptable, just keep it simple
