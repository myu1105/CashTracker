# CashTracker

package program;
import java.util.Scanner;
import java.text.NumberFormat;

public class Main {
	public static double totalSpent, balance, income, needs = 0, wants = 0, misc = 0,
			totalLimit = 0, needsLimit = 0, wantsLimit = 0, miscLimit, score, debt = 0, 
			credit = 0, totalAccrued = 0, totalMisc = 0, totalWants = 0, totalNeeds = 0;
	
	public static void main(String[] args) {
		getAccount();
		getSpending();
		accruedBalance(getDebt(), getCredit());
		System.out.println(summary(calcBalance(), totalSpent, score()));
		
	}
	
	public static void getAccount() {
		Scanner input = new Scanner(System.in);
		System.out.println("Enter balance: ");
		balance = input.nextDouble();
		System.out.println("Enter income: ");
		income = input.nextDouble();
		System.out.println("Enter needs limit: ");
		needsLimit = input.nextDouble();
		System.out.println("Enter wants limit: ");
		wantsLimit = input.nextDouble();
		System.out.println("Enter miscellaneous limit: ");
		miscLimit = input.nextDouble();
		totalLimit = needsLimit + wantsLimit + miscLimit;
		//input.close();
	}
	
	public static void getSpending() {
		Scanner input = new Scanner(System.in);
		String type = "";
		
		System.out.println("Enter number of expenses: ");
		int e = input.nextInt();
		for(int i = 0; i < e; i++) {
			System.out.println("Enter type of spending(needs/wants/misc): ");
			type = input.next();
			System.out.println("Enter amount spent: ");
			if(type.equals("needs")) {
				needs = input.nextDouble();
				totalNeeds += needs;
				totalSpent += needs;
			}else if(type.equals("wants")) {
				wants = input.nextDouble();
				totalWants += wants;
				totalSpent += wants;
			}else if(type.equals("misc")) {
				misc = input.nextDouble();
				totalMisc += misc;
				totalSpent += misc;
			}
		}
	}
	
	public static double calcBalance() {
		//totalSpent = needs + wants + misc;
		balance = balance + income - totalSpent;
		return balance;
	}
	
	public static double getDebt() {
		Scanner input = new Scanner(System.in);
		System.out.println("Enter how much owed to others: ");
		debt = input.nextDouble();
		//totalSpe -= debt;
		return debt;
		//input.close();
	}
	
	public static double getCredit() {
		Scanner input = new Scanner(System.in);
		System.out.println("Enter how much others owe you: ");
		credit = input.nextDouble();
		return credit;
		//input.close();
	}
	public static void accruedBalance (double debt, double credit) {
		totalAccrued = credit - debt;
		//return totalAccrued;
	}

	public static String accruedScore() {
		double acratio;
		int sco = 0;
		String acscoreWarning = "";
		acratio =  ((totalSpent-totalAccrued)/totalLimit);
		
		if(acratio <= .3){
			sco = 5;
			acscoreWarning = "Your accrued finances are in great shape!";
		}else if(acratio >.3 && acratio <=.6){
			sco = 4;
			acscoreWarning= "Your accrued finances are doing well and are halfway through your budgeted monthly allowance";
		}else if(acratio >.6 && acratio <=.9){
			sco = 3;
			acscoreWarning= "Your accrued finances are more than halfway through your budgeted monthly allowance";
		}else if(acratio >.9 && acratio<1){
			sco = 2;
			acscoreWarning= "Be careful with your finances, your accrued finances almost through your budgeted monthly allowance";
		}else if(acratio == 1){
			sco = 1;
			acscoreWarning= "Your accrued finances are through your budgeted monthly allowance";
		}else if(acratio > 1){
			sco = 0;
			acscoreWarning= "You are spending more money than available in your budgeted monthly allowance";
		}
		
		acscoreWarning = "Your Score is: " + sco + "\n" + acscoreWarning;
		
		return acscoreWarning;
		}
    
	public static String score() {
		double ratio;
		int sco = 0;
		String scoreWarning = "";
		ratio =  (totalSpent/totalLimit);
		
		if(ratio <= .3){
			sco = 5;
			scoreWarning = "Your finances are in great shape!";
		}else if(ratio >.3 && ratio <=.6){
			sco = 4;
			scoreWarning= "Your finances are doing well. You are halfway through your budgeted monthly allowance";
		}else if(ratio >.6 && ratio <=.9){
			sco = 3;
			scoreWarning= "You are more than halfway through your budgeted monthly allowance";
		}else if(ratio >.9 && ratio<1){
			sco = 2;
			scoreWarning= "Be careful with your finances, you almost through your budgeted monthly allowance";
		}else if(ratio == 1){
			sco = 1;
			scoreWarning= "You are through your budgeted monthly allowance";
		}else if(ratio > 1){
			sco = 0;
			scoreWarning= "You are spending more money than available in your budgeted monthly allowance";
		}
		
		scoreWarning = "Your Score Before Accrual: " + sco + "\n" + scoreWarning;
		
		return scoreWarning;
		}
	
		public static double accruedBalance() {
			return (balance + totalAccrued);
		}
	
	public static String summary(double balance, double spent, String warning) {
		NumberFormat format = NumberFormat.getCurrencyInstance();
		String summary = "Total Spent: " + format.format(totalSpent) + "\n" + "Spending Breakdown: " +
		format.format(totalNeeds) + "(needs), " + format.format(totalWants) + "(wants), " + format.format(totalMisc) + "(miscellaneous) \n" 
		+ "Balance: " + format.format(balance) + "\n" + score() +
		"\nTotal Accrued (pos/neg is credit/debt): " + format.format(totalAccrued) + "\n" + 
		"Final Balance: "+ format.format(accruedBalance()) + "\n" + accruedScore();
		
		return summary;
	}
}

