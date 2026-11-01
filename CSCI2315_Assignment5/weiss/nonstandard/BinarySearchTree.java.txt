package weiss.nonstandard;

public class BinarySearchTree<AnyType extends Comparable<? super AnyType>>
{
    public BinarySearchTree( )
    {
        root = null;
    }

    public void insert( AnyType x )
    {
        root = insert( x, root );
    }

    public void remove( AnyType x )
    {
        root = remove( x, root );
    }

    public AnyType findMin( )
    {
        return elementAt( findMin( root ) );
    }

    public AnyType findMax( )
    {
        return elementAt( findMax( root ) );
    }

    public AnyType find( AnyType x )
    {
        return elementAt( find( x, root ) );
    }

    /**
     * Exercise 19.16: Nonrecursive findKth using the same technique as nonrecursive find.
     */
    public AnyType findKth( int k )
    {
        BinaryNode<AnyType> t = root;
        while( t != null )
        {
            int leftSize = treeSize( t.left );
            if( k == leftSize + 1 )
                return t.element;
            else if( k <= leftSize )
            {
                t = t.left;
            }
            else
            {
                k -= leftSize + 1;
                t = t.right;
            }
        }
        return null;
    }

    public void makeEmpty( )
    {
        root = null;
    }

    public boolean isEmpty( )
    {
        return root == null;
    }

    /**
     * Prints the tree with '*' characters representing depth levels (Figure 18.6 style).
     */
    public void printTree( )
    {
        if( isEmpty( ) )
            System.out.println( "Empty tree" );
        else
            printTree( root, 0 );
    }

    private AnyType elementAt( BinaryNode<AnyType> t )
    {
        return t == null ? null : t.element;
    }

    protected BinaryNode<AnyType> insert( AnyType x, BinaryNode<AnyType> t )
    {
        if( t == null )
            t = new BinaryNode<AnyType>( x );
        else if( x.compareTo( t.element ) < 0 )
            t.left = insert( x, t.left );
        else if( x.compareTo( t.element ) > 0 )
            t.right = insert( x, t.right );
        else
            ; // Duplicate; do nothing
        return t;
    }

    protected BinaryNode<AnyType> remove( AnyType x, BinaryNode<AnyType> t )
    {
        if( t == null )
            return t;
        if( x.compareTo( t.element ) < 0 )
            t.left = remove( x, t.left );
        else if( x.compareTo( t.element ) > 0 )
            t.right = remove( x, t.right );
        else if( t.left != null && t.right != null )
        {
            t.element = findMin( t.right ).element;
            t.right = remove( t.element, t.right );
        }
        else
            t = ( t.left != null ) ? t.left : t.right;
        return t;
    }

    /**
     * Exercise 19.15: Recursive findMin.
     */
    protected BinaryNode<AnyType> findMin( BinaryNode<AnyType> t )
    {
        if( t == null )
            return null;
        else if( t.left == null )
            return t;
        return findMin( t.left );
    }

    /**
     * Exercise 19.15: Recursive findMax.
     */
    private BinaryNode<AnyType> findMax( BinaryNode<AnyType> t )
    {
        if( t == null )
            return null;
        else if( t.right == null )
            return t;
        return findMax( t.right );
    }

    /**
     * Exercise 19.15: Recursive find.
     */
    private BinaryNode<AnyType> find( AnyType x, BinaryNode<AnyType> t )
    {
        if( t == null )
            return null;
        if( x.compareTo( t.element ) < 0 )
            return find( x, t.left );
        else if( x.compareTo( t.element ) > 0 )
            return find( x, t.right );
        else
            return t;
    }

    private int treeSize( BinaryNode<AnyType> t )
    {
        if( t == null )
            return 0;
        else
            return 1 + treeSize( t.left ) + treeSize( t.right );
    }

    private void printTree( BinaryNode<AnyType> t, int depth )
    {
        if( t != null )
        {
            for( int i = 0; i < depth; i++ )
                System.out.print( "*" );
            System.out.println( t.element );
            printTree( t.left, depth + 1 );
            printTree( t.right, depth + 1 );
        }
    }

    protected BinaryNode<AnyType> root;
}
