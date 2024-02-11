# listOfCars
I utilized the tree function in Java by recreating the application that Brett Alistair Kromkamp (2015) has created. In the Java app that I created, I replaced the names that Kromkamp created with makes of cars. 

<APP.java>
* List of Cars
 * Jamain Hughes
 * CPT307
 * Professor Marquez
 * 10/11/2022
 */

 
package com.quesucede.tree; 
 
import java.util.Iterator; 
 
public class App { 
    public static void main(String[] args) { 
 
        Tree tree = new Tree(); 
 
        /* 
         * The second parameter for the addNode method is the identifier 
         * for the node's parent. In the case of the root node, either 
         * null is provided or no second parameter is provided. 
         */ 
        tree.addNode("Dodge Vehicles"); 
        tree.addNode("2022 Charger $32,945", "Dodge Vehicles"); 
        tree.addNode("2022 Challenger $31,275", "Dodge Vehicles"); 
        tree.addNode("2022 Durango $39,355", "2022 Charger $32,945"); 
        tree.addNode("2023 Hornet $31,590", "2022 Charger $32,945"); 
        tree.addNode("2020 Grand Caravan $27,471", "2022 Challenger $31,275"); 
        tree.addNode("2020 Journey $24,914", "2022 Challenger $31,275"); 
        tree.addNode("2020 Ram $37,090", "2022 Durango $39,355"); 
        tree.addNode("2020 Viper $199,500", "2022 Durango $39,355"); 
        tree.addNode("2005 Neon $5,000", "2023 Hornet $31,590"); 
        tree.addNode("2020 Dakota $30,000", "2023 Hornet $31,590"); 
 
        tree.display("Dodge Vehicles"); 
 
        System.out.println("n***** DEPTH-FIRST ITERATION *****"); 
 
        // Default traversal strategy is 'depth-first' 
        Iterator<Node> depthIterator = tree.iterator("Dodge Vehicles"); 
 
        while (depthIterator.hasNext()) { 
            Node node = depthIterator.next(); 
            System.out.println(node.getIdentifier()); 
        } 
 
        System.out.println("n***** BREADTH-FIRST ITERATION *****"); 
 
        Iterator<Node> breadthIterator = tree.iterator("Dodge Vehicles", TraversalStrategy.BREADTH_FIRST); 
 
        while (breadthIterator.hasNext()) { 
            Node node = breadthIterator.next(); 
            System.out.println(node.getIdentifier()); 
        } 
    } 
} 
<BreadthFirstTreeIterator.java>
/* 
  
 * by Jamain Hughes
 */ 
package com.quesucede.tree;

import java.util.*;

public class BreadthFirstTreeIterator implements Iterator<Node> {
	

    private static final int ROOT = 0;

    private LinkedList<Node> list;
    private HashMap<Integer, ArrayList<String>> levels;

    public BreadthFirstTreeIterator(HashMap<String, Node> tree, String identifier) {
        list = new LinkedList<Node>();
        levels = new HashMap<Integer, ArrayList<String>>();

        if (tree.containsKey(identifier)) {
            this.buildList(tree, identifier, ROOT);

            for (Map.Entry<Integer, ArrayList<String>> entry : levels.entrySet()) {
            for (String child : entry.getValue()) {
                list.add(tree.get(child));
}
}
}
}

    private void buildList(HashMap<String, Node> tree, String identifier, int level) {
        if (level == ROOT) {
            list.add(tree.get(identifier));
}

        ArrayList<String> children = tree.get(identifier).getChildren();

        if (!levels.containsKey(level)) {
            levels.put(level, new ArrayList<String>());
}
        for (String child : children) {
            levels.get(level).add(child);

// Recursive call
            this.buildList(tree, child, level + 1);
}
}

    @Override
    public boolean hasNext() {
        return !list.isEmpty();
}

    @Override
    public Node next() {
        return list.poll();
}

    @Override
    public void remove() {
        throw new UnsupportedOperationException();
}
}

<DepthFirstTreeIterator.java>
package com.quesucede.tree;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.Iterator;
import java.util.LinkedList;

public class DepthFirstTreeIterator implements Iterator<Node> { 
    private LinkedList<Node> list; 
 
    public DepthFirstTreeIterator(HashMap<String, Node> tree, String identifier) { 
        list = new LinkedList<Node>(); 
 
        if (tree.containsKey(identifier)) { 
            this.buildList(tree, identifier); 
        } 
    } 
 
    private void buildList(HashMap<String, Node> tree, String identifier) { 
        list.add(tree.get(identifier)); 
        ArrayList<String> children = tree.get(identifier).getChildren(); 
        for (String child : children) { 
 
            // Recursive call 
            this.buildList(tree, child); 
        } 
    } 
 
    @Override 
    public boolean hasNext() { 
        return !list.isEmpty(); 
    } 
 
    @Override 
    public Node next() { 
        return list.poll(); 
    } 
 
    @Override 
    public void remove() { 
        throw new UnsupportedOperationException(); 
    } 
} 

<Node>
package com.quesucede.tree; 
 
import java.util.ArrayList; 
 
public class Node { 
 
    private String identifier; 
    private ArrayList<String> children; 
 
    // Constructor 
    public Node(String identifier) { 
        this.identifier = identifier; 
        children = new ArrayList<String>(); 
    } 
 
    // Properties 
    public String getIdentifier() { 
        return identifier; 
    } 
 
    public ArrayList<String> getChildren() { 
        return children; 
    } 
 
    // Public interface 
    public void addChild(String identifier) { 
        children.add(identifier); 
    } 
}

<TraversalStrategy>
package com.quesucede.tree;

public enum TraversalStrategy {
	
	DEPTH_FIRST,
	BREADTH_FIRST
	



}

<Tree>
package com.quesucede.tree; 
import java.util.ArrayList;
import java.util.HashMap;
import java.util.Iterator;

public class Tree { 
	 
    private final static int ROOT = 0; 
 
    private HashMap<String, Node> nodes; 
    private TraversalStrategy traversalStrategy; 
 
    // Constructors 
    public Tree() { 
        this(TraversalStrategy.DEPTH_FIRST); 
    } 
 
    public Tree(TraversalStrategy traversalStrategy) { 
        this.nodes = new HashMap<String, Node>(); 
        this.traversalStrategy = traversalStrategy; 
    } 
 
    // Properties 
    public HashMap<String, Node> getNodes() { 
        return nodes; 
    } 
 
    public TraversalStrategy getTraversalStrategy() { 
        return traversalStrategy; 
    } 
 
    public void setTraversalStrategy(TraversalStrategy traversalStrategy) { 
        this.traversalStrategy = traversalStrategy; 
    } 
 
    // Public interface 
    public Node addNode(String identifier) { 
        return this.addNode(identifier, null); 
    } 
 
    public Node addNode(String identifier, String parent) { 
        Node node = new Node(identifier); 
        nodes.put(identifier, node); 
 
        if (parent != null) { 
            nodes.get(parent).addChild(identifier); 
        } 
 
        return node; 
    } 
 
    public void display(String identifier) { 
        this.display(identifier, ROOT); 
    } 
 
    public void display(String identifier, int depth) { 
        ArrayList<String> children = nodes.get(identifier).getChildren(); 
 
        if (depth == ROOT) { 
            System.out.println(nodes.get(identifier).getIdentifier()); 
        } else { 
            String tabs = String.format("%0" + depth + "d", 0).replace("0", "    "); 
// 4 spaces 
            System.out.println(tabs + nodes.get(identifier).getIdentifier()); 
        } 
        depth++; 
        for (String child : children) { 
 
            // Recursive call 
            this.display(child, depth); 
        } 
    } 
 
    public Iterator<Node> iterator(String identifier) { 
    	
        return this.iterator(identifier, traversalStrategy); 
    
    }
 
    public Iterator<Node> iterator(String identifier, TraversalStrategy traversalStrategy) { 
        return traversalStrategy == TraversalStrategy.BREADTH_FIRST ? 
                new BreadthFirstTreeIterator(nodes, identifier) : 
                new DepthFirstTreeIterator(nodes, identifier); 
    } 
 
}
![carlist](https://user-images.githubusercontent.com/108758588/195996087-bb02214b-c13b-49bf-8293-9a61f56e0581.png)
