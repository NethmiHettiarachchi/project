package wekaproj;

/**
 *
 * @author Nethmi Hettiarachchi
 */
import weka.core.*;
import weka.core.FastVector;
import weka.classifiers.meta.FilteredClassifier;
import java.util.List;
import java.util.ArrayList;
import java.io.*;
import java.awt.*;
import javax.swing.*;

public class FinalClassify {


	/**
	 * String that stores the text to classify
	 */
	String text;
	/**
	 * Object that stores the instance.
	 */
	Instances instances;
	/**
	 * Object that stores the classifier.
	 */
	FilteredClassifier classifier;
		
	/**
	 * This method loads the text to be classified.
	 * @param fileName The name of the file that stores the text.
	 */
	public void load(String fileName) {
		try {
			BufferedReader reader = new BufferedReader(new FileReader(fileName));
			String line;
			text = "";
			while ((line = reader.readLine()) != null) {
                text = text + " " + line;
            }
			System.out.println("===== Loaded text data: " + fileName + " =====");
			reader.close();
			//System.out.println(text);
		}
		catch (IOException e) {
			System.out.println("Problem found when reading: " + fileName);
		}
	}
			
	/**
	 * This method loads the model to be used as classifier.
	 * @param fileName The name of the file that stores the text.
	 */
	public void loadModel(String fileName) {
		try {
			ObjectInputStream in = new ObjectInputStream(new FileInputStream(fileName));
            Object tmp = in.readObject();
			classifier = (FilteredClassifier) tmp;
            in.close();
 			System.out.println("===== Loaded model: " + fileName + " =====");
       } 
		catch (Exception e) {
			// Given the cast, a ClassNotFoundException must be caught along with the IOException
			System.out.println("Problem found when reading: " + fileName);
		}
	}
	
	/**
	 * This method creates the instance to be classified, from the text that has been read.
	 */
	public void makeInstance() {
		// Create the attributes, class and text
		FastVector fvNominalVal = new FastVector(4);
		fvNominalVal.addElement("Action");
		fvNominalVal.addElement("Comedy");
                fvNominalVal.addElement("Family-Drama");
                fvNominalVal.addElement("Crime-Adventure");
		Attribute attribute2 = new Attribute("@@class@@", fvNominalVal);
		Attribute attribute1 = new Attribute("text",(FastVector) null);
		// Create list of instances with one element
		FastVector fvWekaAttributes = new FastVector(2);
		fvWekaAttributes.addElement(attribute1);
		fvWekaAttributes.addElement(attribute2);
		instances = new Instances("Test relation", fvWekaAttributes, 1);           
		// Set class index
		instances.setClassIndex(instances.numAttributes()-1);
		// Create and add the instance
		DenseInstance instance = new DenseInstance(2);
		instance.setValue(attribute1, text);
		
		// instance.setValue((Attribute)fvWekaAttributes.elementAt(1), text);
		instances.add(instance);
 		//System.out.println("===== Instance created with reference dataset =====");
		//System.out.println(instances);
	}
	
	/**
	 * This method performs the classification of the instance.
	 * Output is done at the command-line.
	 */

        	public String classify() {
		try {
                      //  System.out.println(instances.instance(0));
			double pred = classifier.classifyInstance(instances.instance(0));
			System.out.println("===== Classified instance =====");
			System.out.println("Class predicted: ");
                        String genre = instances.classAttribute().value((int) pred);
                        return genre;
		}
		catch (Exception e) {
			//System.out.println("Problem found when classifying the text");
                    e.printStackTrace();
                    return null;
		}
                
	}
	
	/**
	 * Main method. It is an example of the usage of this class.
	 * @param args Command-line arguments: fileData and fileModel.
	 */
	public static void main (String[] args) {
	
		FinalClassify classifier;
		
		//System.out.println("Usage: java MyClassifier <fileData> <fileModel>");
            String filename="D:\\Weka-3-7\\data\\teting-plots\\Lucy.txt";
            String modelname="D:\\Weka-3-7\\classifier3.model";
                        
		
                
		classifier = new FinalClassify();
                classifier.load(filename);
		classifier.loadModel(modelname);
		classifier.makeInstance();
		System.out.println(classifier.classify());
		
	}

    
    
}

