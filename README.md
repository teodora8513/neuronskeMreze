# neuronskeMreze
//Ima jedan izlaz i on je pritom 1 ili 0 ------------------------------------------------------------------------
    private double calculateAccuracy(MultiLayerPerceptron network, DataSet test) {
        int TP = 0, TN = 0, FP = 0, FN = 0;

        for (DataSetRow row : test) {
            network.setInput(row.getInput());
            network.calculate();
            double actual = row.getDesiredOutput()[0];
            double predicted = network.getOutput()[0];

            if (actual == 1 && predicted > 0.5)  TP++;  
            if (actual == 0 && predicted <= 0.5) TN++;
            if (actual == 0 && predicted > 0.5)  FP++;
            if (actual == 1 && predicted <= 0.5) FN++;
            
           
        }
        System.out.println("Accuracy: " + (double) (TP + TN) / (TP + TN + FP + FN));
        return (double) (TP + TN) / (TP + TN + FP + FN);

    }

    //Ima jedan izlaz ali izlaz nije 1 ili 0 ------------------------------------------------------------------------
    private double calculateAccuracy1(MultiLayerPerceptron network, DataSet test) {
        int tp = 0, tn = 0, fp = 0, fn = 0;
        for (DataSetRow row : test) {
            network.setInput(row.getInput());
            network.calculate();

            double actual = row.getDesiredOutput()[0];
            double predicted = network.getOutput()[0];

            if (actual > 0.5 && predicted > 0.5)   tp++;	
            if (actual <= 0.5 && predicted <= 0.5) tn++;
            if (actual > 0.5 && predicted <= 0.5)  fn++;
            if (actual <= 0.5 && predicted > 0.5)  fp++;
        }
        double acc = (double) (tp + tn) / (tp + tn + fp + fn);
        System.out.println("Accuracy: " + acc);
        return acc;
    }

    //Ima vise od 1 izlaza
    String[] labels = new String[]{"s1", "s2", "s3"}; // Moze da ih ima 2 ili vise (ne mora striktno 3)
    String[] labels1 = {"s1","s2","s3","s4","s5"};
    
    int output;
    private double calculateAccuracy2(MultiLayerPerceptron network, DataSet test) {
        double sum = 0;
        ConfusionMatrix cm = new ConfusionMatrix(labels);
        for (DataSetRow row : test) {
            network.setInput(row.getInput());
            network.calculate();

            int actual = getMaxIndexArray(row.getDesiredOutput());
            int predicted = getMaxIndexArray(network.getOutput());

            cm.incrementElement(actual, predicted);
        }
        System.out.println(cm);
        for (int i = 0; i < output; i++) {
            int tp = cm.getTruePositive(i);
            int tn = cm.getTrueNegative(i);
            int fp = cm.getFalsePositive(i);
            int fn = cm.getFalseNegative(i);
            sum += (double) (tp + tn) / (tp + tn + fn + fp);
        }

        double acc = (double) sum / output;
        System.out.println("MSNE: " + acc);
        return acc;

    }
    private int getMaxIndexArray(double[] array) {
        int position = 0;
        for (int i = 0; i < array.length; i++) {
            if (array[i] > array[position]) {
                position = i;
            }
        }
        return position;
    }

    
    //Ima jedan izlaz 
    private double calculateMsne(MultiLayerPerceptron network, DataSet test) {
        double msne = 0;
        for (DataSetRow row : test) {
            network.setInput(row.getInput());
            network.calculate();

            double actual = row.getDesiredOutput()[0];
            double predicted = network.getOutput()[0];

            msne += Math.pow((actual - predicted), 2);
        }
        msne = (double) msne / (2 * test.size());
        System.out.println("MSNE: " + msne);
        return msne;
    }    
    
    //Ima vise izlaza
    private double calculateMsne2(MultiLayerPerceptron network, DataSet test) {
        double sum = 0;
        for (DataSetRow row : test) {
            network.setInput(row.getInput());
            network.calculate();
            double[] actual = row.getDesiredOutput();
            double[] predicted = network.getOutput();
            
            for (int i = 0; i < actual.length; i++) {
                sum += Math.pow((actual[i] - predicted[i]), 2);
            }
        }
        double msne = (double) sum / (2 * test.size());
        System.out.println("Accuracy: " + msne);
        return msne;
    }
 


}
